```js
    // 导出,下载
    const handleExport = async (params) => {
        try{
            const response = await Axios.post('/api/xxxx/export_file', {
                ...params
            }, { responseType: 'blob' })
            const blob = new Blob([response.data]);
            FileSaver.saveAs(blob, `${filename}.xlsx`)
         } catch(error){
             if (error.response && error.response.data) {
                const msg = await error.response.data.text();// 报错了打印后端返回的错误
                message.error(msg);
             }
         }
    }

    // 导入,上传
    const handleImport = async ({ file }) => {
        const formData = new FormData()
        formData.append('file', file);
        const { data: { status, msg } } = await Axios.post('/api/xxxxx/excel', formData, {
            headers: { "Content-Type": "multipart/form-data" }
        });
        if (status) {
            message.success(msg);
        } else {
            message.error(msg);
        }
    }



```
