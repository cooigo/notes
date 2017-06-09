## 自定义导出Excel

```
//xxGrid.ts
protected getButtons() {
        var buttons = super.getButtons();
        buttons.push({
            title: "导出",
            cssClass: "export-xlsx-button",
            onClick: () => {
                if (!this.onViewSubmit()) {
                    return;
                }
                Q.postToService({ service: UsersService.baseUrl + '/ListExcel', request: {}, target: '_blank' });
            }
        });

        return buttons;
    }
}



//xxEndpoint.cs
public FileContentResult ListExcelUsersOffer(IDbConnection connection, ListRequest request)
        {
            var uot = connection.Query(@"
select * from xxTable
");

            var data = uot.Select(m => new MyRow
            {
                Id = m.Id,
            }).ToList();

            var report = new DynamicDataReport(data, new List<string> { "Id", }, typeof(Columns.xxColumns));
            var bytes = new ReportRepository().Render(report);
            return ExcelContentResult.Create(bytes, "xxx_" +
                DateTime.Now.ToString("yyyyMMdd_HHmmss") + ".xlsx");
        }


```