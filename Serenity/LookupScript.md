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
