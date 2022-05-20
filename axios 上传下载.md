```js
    const handleExport = async (params) => {
        const response = await Axios.post('/api/xxxx/export_file', {
            ...params
        }, { responseType: 'blob' })
        const blob = new Blob([response.data]);
        FileSaver.saveAs(blob, `${filename}.xlsx`)
    }

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
