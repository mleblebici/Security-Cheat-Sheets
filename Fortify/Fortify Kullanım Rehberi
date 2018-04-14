# FORTIFY SCA
Fortify Source Code Analyzer (SCA) farklı dillerde yazılmış uygulamaların kaynak kod analizini gerçekleştirmektedir. Kaynak kod analizi yapılacak projenin kullandığı dilden bağımsız olarak Fortify SCA iki adımda analizini gerçekleştirmektedir. İlk adım translation aşamasıdır ve bu aşamada Fortify kodu kendi anlayabileceği intermediate bir dile çevirmektedir. Daha sonra ise scan aşaması ile bu kodu taramaktadır. Genellikle taramalarda zaman alan ve uğraştıran kısım translation aşamasıdır. Bu aşamada oluşabilecek problemlerin büyük çoğunluğu uygulamanın kullandığı kütüphanelerin Fortify SCA’ya belirtilmemesinden kaynaklanmaktadır.

## Maven ile Derlenen Java Projelerinin Taranması
Maven ile derlenebilen projeler Fortify maven plugini sayesinde taranabilmektedir. Bunun için öncelikle makine üzerinde maven yüklü olmalıdır. Maveni yüklemen için:
* Apache maven sitesinden zip dosyası indirilir.
* Program Files\Apache\Maven klasörüne bu dosya extract edilir.
* Daha sonar bu dizine ait yol bilgisi M2_HOME ve MAVEN_HOME çevresel değişkenlerine set edilir.
* Komut satırından mvn –version komutu ile çalışıp çalışmadığı kontrol edilebilir.

Maven yüklendikten sonra Fortify maven plugininin yüklenebilmesi için aşağıdaki adımlar takip edilmelidir:
* Komut satırından [Fortify Home]\Samples\advanced\maven-plugin dizinine gidilir.
* Sonrasında mvn clean package install komutu ile maven plugini yüklenir.
Fortify maven plugini yüklendikten sonra, maven ile derlenebilen projenin kaynak kod analizi gerçekleştirilebilir:
* Komut satırından kaynak kod analizi yapılacak projenin bulunduğu dizine gidilir. 
* Bu dizinde öncelikle mvn clean komutu çalıştırılır.
* Daha sonra aşağıdaki komut çalıştırılır.

```
mvn install –Dmaven.test.skip=true –Dfortify.sca.source.version=[the java version of the project, e.g. 1.6, 1.7 or 1.8] –Dfortify.sca.buildId=[project build id] –Dfortify.sca.logfile=[optional logfile] com.fortify.ps.maven.plugin:sca-maven-plugin:translate
```
* Yukarıdaki komut projeyi translate etmektedir. Bundan sonra aşağıdaki komut ile kod taranabilir ve rapor üretilebilir.

```
sourceanalyzer –b [project build id] –scan –f [project report name].fpr
```


### Maven ile Local Repository Üzerinden Projeleri Tarama
Mavene ait settings.xml dosyası içerisinde <repositories> etiketi altına aşağıdakine benzer bir girdi eklenmelidir.
```
<repository>
    <id>name</id>
    <url>file://C:\Users\Fortify\.m2\</url>
</repository>
```

Ayrıca <mirrors> etiketi altına da aşağıdaki girdi eklenmelidir.
```
<mirror>
    <mirrorOf>*</mirrorOf>
    <id>name</id>
    <name>repo</name>
    <url>file://C:\repository</url>
</mirror>
```

Burada önemli olan mirror etiketi içerisinde url kısmında belirtilen dosya yolu altında projenin kullanacağı tüm jar dosyalarının ve repositorylerin tam bir şekilde alınmasıdır.

**NOT**: Fortify maven plugini kullanılamıyorsa, kodun translate aşamasını hızlandırmak için proje maven ile derlenebilir ve sonrasında sourceanalyzer class path değişkenine maven repositorylerinin bulunduğu dizin eklenebilir. Böylece projede kullanılacak jar dosyaları maven aracılığıyla temin edilmiş olur. Maven repository dizini genelde C:\Users\Username\.m2\repository dizini altında bulunmaktadır.

## Python ile Yazılmış Projelerin Taranması
**NOT**: Fortify şu an için sadece 2.7 sürümüne kadar desteklemektedir. Fakat, 3.0 ile yazılan projelere ait kodlar 2.7 versiyonuna convert edilerek analiz edilebilmektedir. Fakat sonuçlar incelenirken bu durum göz önünde bulundurulmalıdır.

Python ile yazılmış projeler taranırken öncelikle projenin hangi python sürümüyle yazıldığının bilinmesi gerekmektedir. Daha sonra ilgili python sürümü analizin yapılacağı makineye yüklenmelidir. SCA python uygulamalarını derlerken projede kullanılan tüm kütüphanelerin lokale indirilmesini gerektirmektedir. Bu yüzden translation aşaması çok uzun sürebilmektedir. Projede kullanılan kütüphanelerin bulundukları dizin bilgisi ve python sürümünün yüklendiği dizin sourceanalyzer komutuna **–python-path** parametresiyle ; ile ayrılarak verilmelidir. Kodun taranması ve raporun oluşturulması aşamasında ayrıca bir parametre verilmesine gerek olmamaktadır, -scan –f parametreleriyle doğrudan yapılabilmektedir.

## Fortify SCA'nın MAC OS Üzerinde Kurulumu
MACOS iso dosyası mount edilir. MACOS klasörü içerisindeki zip dosyası extract edilir ve içindeki Application dosyasına çift tıklanır. SCA kurulum ekranında  lisans sözleşmesi kabul edilir ve lisans dosyasının yol bilgisi sağlanır ve sonrasında next diyerek kurulum tamamlanır. Komut satırından sourceanalyzer komutu çalıştırılarak kurulumun tamamlanıp tamamlanmadığı test edilebilir.

## Android Mobil Uygulama Kaynak Kod Analizi
Android mobil uygulamaları kod analizi MAC OS üzerinde kurulu olan Android Studio’nun Fortify plugini aracılığı ile yapılabilmektedir. Android Studio yüklendikten sonra Android Studio/Preferences/Plugins tabına gidilir. Install Plugin from disk butonuna basılarak Fortify SCA’nın yüklü olduğu dizinde plugins klasörünün altında intellij klasörü altındaki dosya seçilir. Sonrasında OK butonuna basılarak Android Studio uygulaması yeniden başlatılır. Yeniden başlatıldıktan sonra Android Studio üzerinde üst kısımda Fortify sekmesinin geldiği kontrol edilmelidir.

Bundan sonra Android projelerinin kaynak kod analizi yapılırken öncelikle kod Android Studio üzerinden derlenir. Kod derlendikten sonra yukarıdaki Fortify sekmesinden scan Project’e basılarak kodun kaynak kod analizi yapılabilmektedir.

## iOS Uygulamalarının Kaynak Kod Analizi

Fortify SCA, XCode 7.3 ve öncesi sürümleri desteklemektedir. Bu yüzden XCode plugini aracılığıyla kaynak kod analizi yapılmak isteniyorsa, XCode 7.3 önceden makine üzerine yüklenmelidir. Xcode 7.3 Fortify plugini yüklemek için öncelikle XCode 7.3 yüklenmelidir. Yükleme sonrasında FortifyScanningPlugin.xcplugin ile ilgili bir uyarı mesajı çıkacaktır. Bu mesajda Load Bundle seçeneği seçilerek Fortify XCode plugini yüklenebilmektedir. Bu şekilde Plugin yüklendikten sonra XCode ile açılan derlenmiş kod, yukarıdaki menüde bulunan Fortify sekmesi kullanılarak taranabilmektedir. Plugin üzerinden yapılan taramalarda translation aşamasında oluşan uyarılar göz ardı edilmektedir. Bu yüzden bazı durumlarda kodun tamamı analiz edilememektedir. Bu tür durumlardan kaçınmak için her zaman plugin üzerinden taranan projeler –show-loc, -show-files ve –show-build-warnings parametreleriyle komut satırından incelenmelidir. Eğer uyarı çoksa tarama komut satırı üzerinden tekrar yapılmalıdır.
Komut satırı üzerinden iOS uygulamalarının taraması için aşağıdaki komutlar çalıştırılabilir.
```
sourceanalyzer -b buildID -debug -logfile translation.log xcodebuild -project ProjectName.xcodeproj -sdk iphoneos clean build CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO
```

VEYA

```
sourceanalyzer -b buildID -debug -logfile log.log  xcodebuild -scheme SCHEME_NAME -project PROJECT_NAME.xcodeproj -configuration Debug -destination 'platform=iOS Simulator,name=iPhone 6 Plus,OS=10.2' clean build RUN_CLANG_STATIC_ANALYZER=YES CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO
```

```
sourceanalyzer –b buildID –logfile scan.log –scan –f ProjectName.fpr 
```

## Java Uygulamalarının Kaynak Kod Analizi
Java uygulamalarının kaynak kod analizi yapılmadan önce uygulamanın kullandığı harici jar dosyaları elde edilmelidir. Bu dosyalar belirli bir dizinde toplanmalı ve Fortify SCA’ya özel olarak belirtilmelidir. Bu işlem için –cp parametresi kullanılmaktadır. Bu parametreden sonra uygulamanın kullandığı jar dosyalarının yol bilgisi sağlanmaktadır.

```
sourceanalyzer –b JavaProject –cp JarFiles\**\*.jar ProjectPath –logfile translation.log –jdk jdkVersion
```
```
sourceanalyzer –b JavaProject –scan –f JavaProject.fpr –logfile scan.log
```

## ASP.NET Projelerinin Kaynak Kod Analizi
ASP.NET Projelerinin kaynak kod analizi için Visual Studio Developer Command Prompt kullanılmalıdır. Bu sayede aksi halde alınacak pek çok warning ortadan kalkmaktadır. Solution dosyaları bulunan projeler için aşağıdaki komutlar aracılığıyla kaynak kod analizi gerçekleştirilebilmektedir. Öncelikle solution (.sln) dosyasının bulunduğa dizine Developer Command Prompt ile giderek aşağıdaki komutu çalıştırınız.

```
devenv <Solution_file>.sln /Rebuild Debug
```

Proje yukarıdaki komutla başarılı bir şekilde derleniyorsa aşağıdaki komutları çalıştırarak kaynak kod analizini yapabilirsiniz. Eğer derleme esnasında hata veriyorsa, kaynak kod analizine geçmeden önce bu sorunlar ortadan kaldırılmalıdır.

```
devenv <Solution_file>.sln /Clean
```
```
sourceanalyzer –b slnProject –logfile translation.log devenv Project.sln /Rebuild Debug
```
```
sourceanalyzer –b slnProject –logfile scan.log –scan –f slnProject.fpr
```

## Make ile Derlenen Projelerin Kaynak Kod Analizi
Bu projelerin kaynak kod analizi Linux makine üzerinde çok daha kolay yapılabilmektedir. Bunun için Fortify SCA yüklü Linux makine üzerinde aşağıdaki komutlar çalıştırılabilir.

```
./configure
sourceanalyzer –b Project make –logfile translation.log
sourceanalyzer –b Project –scan –f Project.fpr –logfile scan.log
```

## Fortify SCA Yardımcı Komutlar
* Fortify SCA üzerinde bulunan tüm taramaları listelemek için aşağıdaki komut çalıştırılır.

```
sourceanalyzer –show-build-ids
```
* * Fortify SCA üzerinde belirli bir taramada kullanılan dosyaları listelemek için önce Show-build-ids ile ilgili taramanın build id değeri öğrenilmelidir. Sonrasında aşağıdaki komut çalıştırılabilir.

```
sourceanalyzer –b buildID –show-files
```
* Fortify SCA üzerinde belirli bir tarama kapsamında analiz edilen kodun satır sayısını öğrenmek için aşağıdaki komut çalıştırılmalıdır.

```
sourceanalyzer –b buildID –show-loc
```
* Fortify SCA üzerinde belirli bir taramanın translation aşamasında karşılaştığı warning ve error mesajlarını görmek için aşağıdaki komut çalıştırılabilir.

```
sourceanalyzer –b buildID –show-build-warnings
```

* Kaynak kod analizi yapılacak projenin ana makineye kopyalanmadan removable disk üzerinden yapılması istenildiğinde aşağıdaki komut ile intermediate dosyalar disk üzerinde oluşturulabilmektedir.
```
sourceanalyzer -b buildID -Dcom.fortify.sca.ProjectRoot F:\
```

* Kaynak kod analizi yapılacak projenin encoding şeması varsayılandan farklı ise (örn. Türkçe karakterler içeriyorsa), aşağıdaki komut anahtarı ile encoding şeması ayarlanabilmektedir.
```
sourceanalyzer -b buildID -encoding UTF-8
```
