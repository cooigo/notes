## 自定义下拉选项数据

```
namespace PlayMoboAdmin.ResourceManager.Entities
{
    [LookupScript("PlayMobo.PushPage")]
    public class PushPageLookupScript: LookupScript
    {
        public PushPageLookupScript() : base()
        {
            this.IdField = "PageName";
            this.TextField = "Desc";
            this.getItems = GetItems;
        }

        protected List<PushPage> GetItems()
        {
            var pages = PlayMobo.HttpService.AdminService.GetPushMessageUrl();

            return pages.Select(m => new Entities.PushPage { PageName = m.PageName, Desc = m.Desc, Params = m.Params }).ToList();
        }
    }
}
```

## 根据条件过滤数据

```
//Editor
namespace PlayMoboAdmin.ResourceManager {

    @Serenity.Decorators.registerEditor()
    export class AppsCategoryEditor extends Serenity.LookupEditorBase<Serenity.LookupEditorOptions, CategoryRow> {

        constructor(container: JQuery, opt: Serenity.LookupEditorOptions) {
            super(container, opt);
        }

        /**
         * Normally LookupEditor requires a lookup key to determine which set of
         * lookup data to show in editor. As our editor will only show category
         * data, we lock it to category lookup key.
         */
        protected getLookupKey() {
            return CategoryRow.lookupKey;
        }

        /**
         * Here we are filtering by category name but you could filter by any field.
         * Just make sure the fields you filter on has [LookupInclude] attribute on them,
         * otherwise their value will be null in client side as they are not sent back
         * from server in lookup script.
         */
        protected getItems(lookup: Q.Lookup<CategoryRow>) {
            var items = super.getItems(lookup);
            if (Q.isEmptyOrNull(this._platform)) {
                return items.filter(x =>
                    x.ResType === Extensions.ResTypeOption.App);
            }
            else {
                return items.filter(x =>
                    x.ResType === Extensions.ResTypeOption.App && x.Platform == Q.toId(this._platform));
            }
        }

        private _platform: string;

        get platform() {
            return this._platform;
        }

        set platform(value: string) {
            if (this._platform !== value) {
                this._platform = value;
                this.updateItems();
            }
        }

    }
}

//Dialog

namespace PlayMoboAdmin.ResourceManager {

    @Serenity.Decorators.registerClass()
    @Serenity.Decorators.responsive()
    export class AppsDialog extends Serenity.EntityDialog<AppsRow, any> {
        protected getFormKey() { return AppsForm.formKey; }
        protected getIdProperty() { return AppsRow.idProperty; }
        protected getLocalTextPrefix() { return AppsRow.localTextPrefix; }
        protected getNameProperty() { return AppsRow.nameProperty; }
        protected getService() { return AppsService.baseUrl; }

        protected form = new AppsForm(this.idPrefix);


        constructor() {
            super();

            this.form.Platform.changeSelect2(e => {
                var pt = this.form.Platform.value;
                this.form.CategoryId.platform = pt;
            });

        }

        protected afterLoadEntity() {
            super.afterLoadEntity();
            this.tagGrid.appID = this.entityId;
            this.form.CategoryId.platform = this.form.Platform.value;
        }

    }
}


```
