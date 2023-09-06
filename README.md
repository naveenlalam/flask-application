const exportToExcel = () => {
    const worksheet = XLSX.utils.json_to_sheet([]);
    const workbook = XLSX.utils.book_new();

    // Add data from API 1 with column names
    XLSX.utils.sheet_add_json(worksheet, data1, { origin: 'A1' });

    // Add an empty row for spacing
    XLSX.utils.sheet_add_json(worksheet, [{}], { origin: 'A' + (data1.length + 2) });

    // Add data from API 2 with column names
    XLSX.utils.sheet_add_json(worksheet, data2, { origin: 'A' + (data1.length + 3) });

    const worksheetName = 'Combined Data';
    XLSX.utils.book_append_sheet(workbook, worksheet, worksheetName);

    XLSX.writeFile(workbook, 'combined_data.xlsx');
  };
