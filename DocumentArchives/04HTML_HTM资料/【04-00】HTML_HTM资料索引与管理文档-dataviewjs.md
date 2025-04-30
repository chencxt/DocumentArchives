查询某个文件，需要将下面的dataviewjs的输出内容复制下来，并搜索。


```dataviewjs
// 获取当前文件夹路径
const currentFolder = app.workspace.getActiveFile().parent.path;

// 使用 dataviewjs 列出当前文件夹及其子文件夹内的 .html 和 .htm 文件，并按创建日期排序
const htmlFiles = app.vault.getFiles()
    .filter(file => 
        (file.extension === "html" || file.extension === "htm") && // 筛选 .html 和 .htm 文件
        file.path.startsWith(currentFolder) // 限制文件路径在当前文件夹及其子文件夹内
    )
    .sort((a, b) => a.stat.ctime - b.stat.ctime); // 按创建日期升序排序

// 准备表格数据
const tableData = htmlFiles.map(file => {
    const creationDate = new Date(file.stat.ctime); // 将 Unix 时间戳转换为 JavaScript Date 对象
    const formattedDate = creationDate.toISOString().split("T")[0]; // 格式化为 YYYY-MM-DD 格式
    return [dv.fileLink(file.path), formattedDate]; // 返回文件名和创建日期
});

// 输出表格
dv.table(["File Name", "Creation Date"], tableData);


```