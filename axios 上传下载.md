```js
const handleExport = async () => {
        const response = await Axios.post('/api/business_database/export_file', {
            workType: workTypeRef.current,
            ...params
        }, { responseType: 'blob' })
        const blob = new Blob([response.data]);
        FileSaver.saveAs(blob, `${workTypeName}.xlsx`)
    }

    const handleImport = async ({ file }) => {
        const formData = new FormData()
        formData.append('file', file);
        const { data: { status, msg } } = await Axios.post('/api/business_database/projects/excel', formData, {
            headers: { "Content-Type": "multipart/form-data" }
        });
        if (status) {
            message.success(msg);
            queryList({
                workType: workTypeRef.current,
                page: pagination.current,
                per_page: pagination.pageSize
            });
        } else {
            message.error(msg);
        }
    }



```
