## grid 常用自定义调整

> [删除/增加]按钮

```
/**
* This method is called to get list of buttons to be created.
*/
protected getButtons(): Serenity.ToolButton[] {

    // call base method to get list of buttons
    // by default, base entity grid adds a few buttons, 
    // add, refresh, column selection in order.
    var buttons = super.getButtons();

    // here is several methods to remove add button
    // only one is enabled, others are commented

    // METHOD 1
    // we would be able to simply return an empty button list,
    // but this would also remove all other buttons
    // return [];

    // METHOD 2
    // remove by splicing (something like delete by index)
    // here we hard code add button index (not nice!)
    // buttons.splice(0, 1);

    // METHOD 3 - recommended
    // remove by splicing, but this time find button index
    // by its css class. it is the best and safer method
    buttons.splice(Q.indexOf(buttons, x => x.cssClass == "add-button"), 1);

    buttons.push(PuTaoTecCom.Admin.Common.ExcelExportHelper.createToolButton({
        grid: this,
        onViewSubmit: () => this.onViewSubmit(),
        service: 'TaskAdmin/MachineActiveException/ListExcel',
        separator: true,
    }));

    return buttons;
}
```

> 设置列表请求地址

```
protected getViewOptions(): Slick.RemoteViewOptions {
    let opt = super.getViewOptions();
    opt.url = Q.resolveUrl("~/Services/" + this.getService() + "/List"); 
    opt.rowsPerPage = 1;
    return opt;
}
```

> 启用分页

```
protected usePager() {
    return false;
}
```

> 列设置

```
protected getColumns(): Slick.Column[] {
    let fld = MachineActiveExceptionRow.Fields;
    return super.getColumns().filter(x => x.field !== fld.ErrorCount);
}
```

> Excel导出

```
public FileContentResult ListExcel(IDbConnection connection, ListRequest request)
{
    var data = List(connection, request).Entities;
    var report = new DynamicDataReport(data, request.IncludeColumns, typeof(Columns.CustomStatusStatDetailColumns));
    var bytes = new ReportRepository().Render(report);
    return ExcelContentResult.Create(bytes, "StatDetailList_" +
        DateTime.Now.ToString("yyyyMMdd_HHmmss") + ".xlsx");
}
```

> 去除id列表

```
protected getIdProperty() { return "__id"; }

private nextId = 1; 
protected onViewProcessData(response: Serenity.ListResponse<TaskAdmin.CustomStatusStatDetailRow>) {
    response = super.onViewProcessData(response);
    for (var x of response.Entities) {
        (x as any).__id = this.nextId++;
    }
    return response;
}

```

> 自定义搜索控件/设置初始值

```
protected createQuickFilters(): void {

    //super.createQuickFilters();
    let fld = xxxRow.Fields;

    let d = new Date();
    if (d.getDate() > 2) {
        d.setDate(d.getDate() - 2);
    }
    else {
        d.setDate(-2);
    }
    //this.findQuickFilter(Serenity.StringEditor, fld.Time).value = Q.formatDate(d, "yyyyMMdd");

    this.addQuickFilter({
        type: Serenity.DateEditor,
        field: "Time"
    });
    this.findQuickFilter(Serenity.DateEditor, "Time").valueAsDate = d;
}

```

> 设置自定义检索条件

```
protected onViewSubmit() {
    // only continue if base class returns true (didn't cancel request)
    if (!super.onViewSubmit()) {
        return false;
    }

    // view object is the data source for grid (SlickRemoteView)
    // this is an EntityGrid so its Params object is a ListRequest
    var request = this.view.params as Serenity.ListRequest;

    // list request has a Criteria parameter
    // we AND criteria here to existing one because 
    // otherwise we might clear filter set by 
    // an edit filter dialog if any.

    request.Criteria = Serenity.Criteria.and(request.Criteria,
        [['UnitsInStock'], '>', 10],
        [['CategoryName'], '!=', 'Condiments'],
        [['Discontinued'], '=', 0]);

    // TypeScript doesn't support operator overloading
    // so we had to use array syntax above to build criteria.

    // Make sure you write
    // [['Field'], '>', 10] (which means field A is greater than 10)
    // not 
    // ['A', '>', 10] (which means string 'A' is greater than 10

    return true;
}
```


> 列自定义格式输出

```
//InlineImageFormatter.ts

namespace Serene1.BasicSamples {

    @Serenity.Decorators.registerFormatter()
    export class InlineImageFormatter
        implements Slick.Formatter, Serenity.IInitializeColumn {
            
        format(ctx: Slick.FormatterContext): string {

            var file = (this.fileProperty ? ctx.item[this.fileProperty] : ctx.value) as string;
            if (!file || !file.length)
                return "";

            let href = Q.resolveUrl("~/upload/" + file);

            if (this.thumb) {
                var parts = file.split('.');
                file = parts.slice(0, parts.length - 1).join('.') + '_t.jpg';
            }

            let src = Q.resolveUrl('~/upload/' + file);

            return `<a class="inline-image" target='_blank' href="${href}">` +
                `<img src="${src}" style='max-height: 145px; max-width: 100%;' /></a>`;
        }

        initializeColumn(column: Slick.Column): void {
            if (this.fileProperty) {
                column.referencedFields = column.referencedFields || [];
                column.referencedFields.push(this.fileProperty);
            }
        }

        @Serenity.Decorators.option()
        public fileProperty: string;

        @Serenity.Decorators.option()
        public thumb: boolean;
    }
}

//在设置属性前，要先t4编译下才能有智能提示

[InlineImageFormatter(FileProperty = "ProductImage", Thumb = true), Width(450)]
public String ProductThumbnail { get; set; }

```





