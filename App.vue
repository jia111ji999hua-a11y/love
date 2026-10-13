<script>
export default {
  onLaunch() {
    if (wx && wx.cloud) wx.cloud.init({ env: 'cloud1-d6g55aa7r075a4fb5', traceUser: true })
  }
}
</script>
<style>
page{background:#f5f6f8;color:#1f2937;font-size:28rpx}.page{padding:24rpx}.card{background:#fff;border-radius:16rpx;padding:24rpx;margin-bottom:20rpx}.btn{background:#1677ff;color:#fff;border-radius:10rpx;padding:20rpx;text-align:center}.muted{color:#6b7280}.tag{display:inline-block;padding:6rpx 14rpx;background:#eaf2ff;color:#1677ff;border-radius:20rpx;font-size:22rpx}
</style>
