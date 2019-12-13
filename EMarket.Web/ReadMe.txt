EMarket.Web olarak ASP core 2.2 seçilerek mvc individual seçilir.
class lib EMarket.ApplicationCore olarak açýlýr classý sil .Net Core 2.2'ye çek
class lib EMarket.Infrastructure olarak açýlýr classý sil .Net Core 2.2ye çek

Referanslar verilir Bunu Web yapar diðer ikisine referans verir(Dependenciese sað týklla)
Infrastructure 'dan ApplicationCore referans verir

ApplicationCore içerisine Interfaces,Services ve entites klasörleri açýlýr
Infrastructure içerisine data ve migrations diye klasörleri açýlýr

ApplicationCore-Entities AppUser classý acýlýr.
public class ApplicationUser : IdentityUser'den miras alýrýz ve Identityuser'ý kullanmak ve bunun için 
install-package microsoft.aspnetcore.identity.entityframeworkcore -version 2.2.0 kurulumu yapýlýr.

EMarket.Web Data-->ApplicationDbContext classý kopyalanýr
ve Infrastructure.dataya ApplicationDbContext classý acýlarak buraya yapýstýrýlýr
ve IdentityDbContext kullanmak için 
infrastructure'a install-package microsoft.entityframeworkcore.sqlserver -version 2.2.0 kurulumu yapýlýr.
Bu class public class ApplicationDbContext : IdentityDbContext<ApplicationUser> haline getirilir.

EMarket.Web içerisinde Data klasörü silinir.

startupa <ApplicationDbContext> ctrl+.+enter namespace'den data silinir.
services.AddDefaultIdentity<IdentityUser>() yerine
services.AddDefaultIdentity<ApplicationUser>() bu hale gelir.

appsettings.json'a gel.
    "DefaultConnection": "Server=.;Database=EMarketDb; uid=sa pwd=123" güncelleþtirilir.

    EMarket.Infrastructure seçiliyken
    PM> add-migration Identity
    sonra update-database

    startup daha sonra ConfigureServices içi services.AddIdentity<ApplicationUser, IdentityRole>() haline getirilir.

    _ViewImports'da @using EMarket.ApplicationCore.Entities eklenir ve _LoginPartial'da
    @inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager haline getirilir.
EMarket.ApplicationCore Entities'e BaseEntity ve Category Class'ý acýlýr

Infrastructure
ApplicationDbContext'e
public DbSet<Category> Categories { get; set; } eklenir.
 daha sonra
 override OnModelCreating oluþturulur.

 EMarket.Infrastructure->Data->Config klasörü aç CategoryConfiguration classý aç

 public class CategoryConfiguration : IEntityTypeConfiguration<Category> haline gelir ve Configura olarak implement edilir.

 Infrastructure'da
 add-migration CategoryEntity

 An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
hatasý alýndýðý için migrations klasörü silinir veri tabaný ssms'den silinir.

clean solution ve yeniden
add-migration Identity denir 0'dan add-migrations olmus olur.
update-database
