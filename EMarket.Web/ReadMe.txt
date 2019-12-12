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