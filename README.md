# COS_upload
##腾讯云上传
``` bush
1. npm 安装 npm i cos-js-sdk-v5 --save

2.引入COS 
<script>
  var COS = require('cos-js-sdk-v5');
</script>
3.创建上传对象, 添加密钥--> (此方法极其不推荐, 本地自己开发可以暂时这么写)
```
## 上传对象
``` bush
var cos = new COS({
    SecretId: 'COS_SECRETID',  PS: ==>('COS_SECRETID' ) ==> 自己腾讯云管理后台可以查看
    SecretKey: 'COS_SECRETKEY',     ==>('COS_SECRETKEY') ==> 自己腾讯云管理后台可以查看
});
              ||
              ||
              \/
cos.putObject({
    Bucket: 'XXXX-1258210079', /* 必须 */
    Region: 'ap-beijing', /* 存储桶所在地域，必须字段 */
    Key: 'admin/' + fileName, /* 必须 ---- admin/----是文件夹 */
    StorageClass: 'STANDARD',
    Body: params.file, // 上传文件对象 params,file ====>  (file blod)
    onProgress: function(progressData) { /* 非必须 */
      console.log('2成功获取Progress：', JSON.stringify(progressData))
    }
  }, function(err, data) {
    // console.log('2获取：', err || data)
    console.log('3.获取失败：', err)
    console.log('3.获取成功: ', data)
    if (data.statusCode === 200) {
      _this.imageUrl = 'https://' + data.Location // 显示预览图片
    }
  })
```
## 云点播 tcVod
``` bush
const tcVod = new TcVod({
                getSignature: getSignature
            })
            console.log('1.上传视频', e.target.files[0])            
            var uploader = tcVod.upload({
                mediaFile: e.target.files[0]
            });
            const _this = this
            uploader.on("media_progress", function(info) {
                uploaderInfo.progress = info.percent;
                console.log('2.进度参数', info.percent)
                console.log('2.进度进程', (uploaderInfo.progress * 100) + '%')
                if((uploaderInfo.progress * 100) < 100){
                    console.log('2-1判断进程开始', (uploaderInfo.progress * 100))
                    // if(this.myIntervalStatus){
                        _this.current = parseInt((uploaderInfo.progress * 100) * _this.videoSize / 100) 
                        console.log('2-2开启', _this.videoSize)
                        _this.myIntervalFuntion()
                    // } else {
                    //     console.log('2-2不开启', this.myIntervalStatus)
                    // }
                } else if ((uploaderInfo.progress * 100) == 100) {
                    console.log('2-1判断进程完成', (uploaderInfo.progress * 100))
                    // clearInterval(this.myInterval)
                    // this.myIntervalStatus = true
                    _this.altime = -1
                    _this.speed = 0
                    _this.Surplus = 0
                } else {
                    console.log('2-1判断进程出错', (uploaderInfo.progress * 100))
                }
            });
            uploader.on("media_upload", function(info) {
                uploaderInfo.isVideoUploadSuccess = true;
                console.log('3,上传结果', info,uploaderInfo.isVideoUploadSuccess)
            });

            console.log(uploader, "uploader");

            var uploaderInfo = {
                videoInfo: uploader.videoInfo,
                isVideoUploadSuccess: false,
                isVideoUploadCancel: false,
                progress: 0,
                fileId: "",
                videoUrl: "",
                cancel: function() {
                uploaderInfo.isVideoUploadCancel = true;
                uploader.cancel();
                }
            }
            console.log('4.uploaderInfo:', uploaderInfo)

            this.uploaderInfos.push(uploaderInfo);
            console.log('5.uploaderInfos[]:', uploaderInfo)


            uploader
                .done()
                .then(function(doneResult) {
                console.log("6.上传成功结果doneResult", doneResult);

                uploaderInfo.fileId = doneResult.fileId;

                return getAntiLeechUrl(doneResult.video.url);
            }).then(function(videoUrl) {
                uploaderInfo.videoUrl = videoUrl;
                console.log('7，视频地址：', uploaderInfo.videoUrl)
                _this.video_url = uploaderInfo.videoUrl
                console.log('8.判断表单是否获取video_url: ',_this.adminVideoId, '||', _this.video_url, '||',)
            });
```
