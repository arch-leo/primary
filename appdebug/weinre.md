## 移动端bug测试 (node)
* 新建项目 npm init
* 然后 npm i --save wernre
* 启动 weinre --boundHost 172.16.28.162 --httpPort 8888 //(boundHost可选hostname | ip dress) (httpPort未被占用的端口号)
* 浏览器打开 http://172.16.28.162:8888
* 在需要 测试的页面 上添加js
> <script src="http://172.16.28.162:8888/target/target-script-min.js#anonymous"></script>
* 浏览器打开 http://172.16.28.162:8888/client/#anonymous 进入页面调试界面
* 在bug手机上刷新 测试的页面，调试界面便可捕获异常 css html or console ...
