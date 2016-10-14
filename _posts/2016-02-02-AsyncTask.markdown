---
layout: post
title:  "Using Async Task download file"
date:   2016-02-02 10:00:00 +0800
tags: 
    - android
---

不管是开发Windows的Winform，WPF还是Android的原生App，如果涉及到了UI的更新，都是推荐新开后台线程。
而且在Android开发中，如果不用后台线程或者Handler去更新UI，貌似连编译都通不过的。

AsyncTask就是专门设计用来执行后台任务，同时提供内部方法来更新UI，还有进度更新和提供Cancel的方法。
对于像后台文件下载这种处理，再适合不过了。

AsyncTask有三个参数类型需要指定

* Params 执行参数类型
* Progress 进度参数类型
* Result 结果参数类型

AsyncTask有4个方法，也是执行步骤可以使用

* onPreExecute 执行前
* doInBackground 后台任务执行，在任务执行时可以调用isCancelled()来判断是否取消任务，如果任务取消onPostExecute就不调用了，调用onCancelled
* onProgressUpdate 执行中
* onPostExecute 执行结束

示例代码如下，完整代码参考 [File downloader GitHub]

{% highlight java %}
public class DownloadTask extends AsyncTask<FileItem, Integer, Integer> {

    private static final String LOG_TAG = "DownloadTask";

    private FilesAdapter filesAdapter;

    public DownloadTask(FilesAdapter filesAdapter) {
        this.filesAdapter = filesAdapter;
    }

    @Override
    protected Integer doInBackground(FileItem... params) {
        System.out.println("DownloadTask doInBackground");

        FileItem fileItem = params[0];

        final DownloadTask me = this;
        fileItem.setOnCancelListener(new FileItem.OnCancelListener() {
            @Override
            public void onCancel() {
                me.cancel(true);
            }
        });

        InputStream inputStream = null;
        OutputStream outputStream = null;
        HttpURLConnection conn = null;
        long total = 0;

        try {

            URL url = fileItem.getUrl();
            conn = (HttpURLConnection) url.openConnection();
            conn.connect();

            int responseCode = conn.getResponseCode();
            Log.d(LOG_TAG, "response code is: " + responseCode);
            Log.d(LOG_TAG, "response content length is: " + conn.getContentLength() / 1024 + " k");

            inputStream = conn.getInputStream();

            File dir = new File(DownloadManager.DL_DIR);
            if (!dir.exists()) {
                dir.mkdir();
            }

            File file = new File(DownloadManager.DL_DIR + fileItem.getFileName());  //TODO check file exist
            Log.d(LOG_TAG, "File download start: " + file.getPath());
            file.createNewFile();

            fileItem.setStatus(FileItem.STATUS_STARTED);

            outputStream = new FileOutputStream(file);
            byte[] buffer = new byte[1024 * 4];
            int count;
            while ((count = inputStream.read(buffer)) != -1) {
                total += count;

                outputStream.write(buffer, 0, count);

                fileItem.setFileSizeDownload(total);
                publishProgress(fileItem.getPosition());

                if (isCancelled()) {
                    Log.d(LOG_TAG, "doInBackground: task cancelled.");
                    fileItem.setStatus(FileItem.STATUS_PENDING);

                    try {
                        if (outputStream != null) {
                            outputStream.flush();
                            outputStream.close();
                        }
                        if (inputStream != null)
                            inputStream.close();
                    } catch (IOException ignored) {
                    }

                    if (conn != null)
                        conn.disconnect();

                    return fileItem.getPosition();
                }
            }

            Log.d(LOG_TAG, "File download complete, size: " + String.valueOf(total / 1024) + " k");
            fileItem.setStatus(FileItem.STATUS_COMPLETE);
            fileItem.setFileSizeDownload(total);
        } catch (Exception e) {
            e.printStackTrace();
            fileItem.setStatus(FileItem.STATUS_ERROR);
        } finally {
            try {
                if (outputStream != null) {
                    outputStream.flush();
                    outputStream.close();
                }
                if (inputStream != null)
                    inputStream.close();
            } catch (IOException ignored) {
            }

            if (conn != null)
                conn.disconnect();
        }

        return fileItem.getPosition();
    }

    @Override
    protected void onPreExecute() {
        System.out.println("onPreExecute");
        filesAdapter.notifyDataSetChanged();
    }

    @Override
    protected void onProgressUpdate(Integer... values) {
        filesAdapter.notifyItemChanged(values[0]);
    }

    @Override
    protected void onPostExecute(Integer i) {
        System.out.println("Downloaded and result position: " + i);
        filesAdapter.notifyItemChanged(i);
    }
}
{% endhighlight %}

[File downloader GitHub]: https://github.com/celery94/FileDownloader