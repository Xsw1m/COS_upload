# COS_upload
腾讯云上传
-------------------------------------
1. npm 安装 npm i cos-js-sdk-v5 --save

2.引入COS 
<script>
  var COS = require('cos-js-sdk-v5');
</script>
3.创建上传对象, 添加密钥--> (此方法极其不推荐, 本地自己开发可以暂时这么写)

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
