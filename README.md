# Proyecto final de Análisis de datos
### Integrantes:
### - Raúl Tenorio
### - Ariel Calderón
### - Mateo Cueva
### - Leonel Molina

La aplicación desarrollada consiste en un sistema capaz de recolectar información de distintas fuentes basándose en un tema específico, para después almacenar dicha información en bases de datos relacionales (MS SQL Server, MySQL, SQLite) y no relacionales (Mongo DB, Couch DB).

## Componentes necesarios

El programa está desarrollado en su totalidad con Python, por lo que se necesita un intérprete de Python 8 o superior. Además, con el administrador de paquetes [pip](https://pip.pypa.io/en/stable/) deben descargarse los siguientes módulos:

```bash
pip install tweepy
pip install couchdb
pip install pymongo
pip install pandas
pip install facebook_scraper
pip install tiktok-scraper
pip install pymysql
pip install pyodbc
pip install Pillow
```

## Uso
Se inicia el programa y a través de la interfaz gráfica inicial se puede establecer el número de datos que se desea recolectar por cada fuente en el campo 'Límite de datos'.
>Advertencia: No se recomienda ingresar un numero muy alto, porque algunas funciones podrian tomar varios minutos en ejecutarse.

![This is an image](/Capturas_proyecto/interfaz.png)

Después, se oprimen los botones 'Buscar' para comenzar la recolección de datos de cada uno de los temas.</br>

Interfaz gráfica
```
import tkinter as tk
from PIL import ImageTk, Image

title_size = 18
text_size = 10
text_font = 'Helvaltical'

ventana = tk.Tk()
ventana.geometry('840x550')
ventana.resizable(width=False, height=False)
ventana.title('Proyecto Final de Analisis de Datos')

def label_text(text,x, y, font, size):
    global tk
    label = tk.Label(ventana, text = text)
    label.pack()
    label.place(x=x,y=y)
    label.config(font=(font, size))

def place_button(text, function, x, y):
    global tk
    button = tk.Button(ventana, text=text, command=function)
    button.pack()
    button.place(x=x, y=y)

def recolectar_pulso_politico():
    for ciudad in ciudades:
        save2cdb(generateTwitterDataByCity(ciudad), 'pulso_twitter')
    cdb2mdb('pulso_twitter', 'pulso_twitter')
    
    label_text('Recoleccion completa!',500, 135, text_font, text_size)

def recolectar_estado_covid():
    covid = generateWebScrapingCovid()
    save2sqlite(covid, "covid_webscraping")
    sqlite2mdb("covid_webscraping", "covid_webscraping")
    
    facebook = generateFacebookData("coronavirus")
    save2mysql(facebook, "covid_facebook")
    mysql2mdb("covid_facebook", "covid_facebook")
    
    label_text('Recoleccion completa!',500, 195, text_font, text_size)
 
def recolectar_juegos():
    for pais in paises:
        save2cdb(generateTwitterDataByCountry(pais), 'juegos_twitter')
    cdb2mdb('juegos_twitter', 'juegos_twitter')
    
    tiktok = generateTikTokData("videojuegos")
    save2mdbAtlas(tiktok, "juegos_tiktok")
    mdbatlas2mdb("juegos_tiktok", "juegos_tiktok")
    
    csv2sqlserver('JuegosDS1','juegos_dataset1')
    csv2sqlserver('JuegosDS2','juegos_dataset2')
    sqlserver2mdb('juegos_dataset1', 'juegos_dataset1')
    sqlserver2mdb('juegos_dataset2', 'juegos_dataset2')
    
    label_text('Recoleccion completa!',500, 255, text_font, text_size)

def recolectar_peliculas():
    peliculas1980 = generateWebScrapingMovies(1980)
    peliculas1990 = generateWebScrapingMovies(1990)
    peliculas2000 = generateWebScrapingMovies(2000)
    peliculas2010 = generateWebScrapingMovies(2010)
    peliculas2020 = generateWebScrapingMovies(2020)

    save2sqlite(peliculas1980, "peliculas_webscraping")
    save2sqlite(peliculas1990, "peliculas_webscraping")
    save2sqlite(peliculas2000, "peliculas_webscraping")
    save2sqlite(peliculas2010, "peliculas_webscraping")
    save2sqlite(peliculas2020, "peliculas_webscraping")
    sqlite2mdb("peliculas_webscraping", "peliculas_webscraping")
    
    csv2sqlserver('MoviesDS3','peliculas_dataset3')
    csv2sqlserver('MoviesDS4','peliculas_dataset4')
    sqlserver2mdb('peliculas_dataset3', 'peliculas_dataset3')
    sqlserver2mdb('peliculas_dataset4', 'peliculas_dataset4')
    
    label_text('Recoleccion completa!',500, 315, text_font, text_size)

def recolectar_noticias():
    facebook = generateFacebookData("cnn")
    save2mysql(facebook, "eventos_facebook")
    mysql2mdb("eventos_facebook", "eventos_facebook")

    tiktok = generateTikTokData("cnn")
    save2mdbAtlas(tiktok, "eventos_tiktok")
    mdbatlas2mdb("eventos_tiktok", "eventos_tiktok")
    
    label_text('Recoleccion completa!',500, 380, text_font, text_size)
    
def convertir_csv():
    mdb2csv('peliculas_webscraping', 'peliculas_webscraping')
    mdb2csv('covid_webscraping', 'covid_webscraping')
    mdb2csv('juegos_twitter', 'juegos_twitter')
    mdb2csv('pulso_twitter', 'pulso_twitter')
    mdb2csv('covid_facebook', 'covid_facebook')
    mdb2csv('eventos_facebook', 'eventos_facebook')
    mdb2csv('juegos_tiktok', 'juegos_tiktok')
    mdb2csv('eventos_tiktok', 'eventos_tiktok')
    mdb2csv('juegos_dataset1', 'juegos_dataset1')
    mdb2csv('juegos_dataset2', 'juegos_dataset2')
    mdb2csv('peliculas_dataset3', 'peliculas_dataset3')
    mdb2csv('peliculas_dataset4', 'peliculas_dataset4')
    
def cambiar_limite():
    limit = input.get()
    print(limit)
 
#Texto de etiquetas

label_text('Recolector de Datos', 313, 15, text_font , title_size)

label_text('Limite de datos', 67, 69, text_font, text_size)

label_text('Pulso politico en Ecuador', 127, 142, text_font, text_size)

label_text('Estado del COVID a nivel mundial', 127, 201, text_font, text_size)

label_text('Juegos en linea', 127, 260, text_font, text_size)

label_text('Peliculas taquilleras por decada', 127, 319, text_font, text_size)

label_text('Noticias mundiales', 127, 378, text_font, text_size)

#Input

input = tk.Entry(ventana)
input.pack()
input.place(x=200, y=70)

#Botones
place_button('Cambiar', cambiar_limite, 400, 70)

place_button('Buscar', recolectar_pulso_politico, 400, 135)

place_button('Buscar', recolectar_estado_covid, 400, 195)

place_button('Buscar',recolectar_juegos, 400, 255) 

place_button('Buscar', recolectar_peliculas, 400, 315)

place_button('Buscar', recolectar_noticias, 400, 375)

place_button('Obtener CSV',convertir_csv, 300, 500)

#Imagenes

logo = ImageTk.PhotoImage(Image.open('EPN_logo.png'))
logo_epn=tk.Label(image=logo)
logo_epn.pack()
logo_epn.place(x=695, y=15)


ventana.mainloop()
          
```
Ejecución final
```
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726426933141516 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726440464007170 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726464363147271 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726489533124615 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726509195976713 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726545636036608 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream encountered HTTP error: 420
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726822137237504 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726850641727497 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726872271753219 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726892186263559 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726914332282882 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498726934590722051 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream encountered HTTP error: 420
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727207925174275 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727238409281538 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727266209058818 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727287465787392 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727314301255680 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727338514780169 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream encountered HTTP error: 420
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727614701318145 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727640429215744 guardado en CouchDB[pulso_twitter]
Envio de los datos a CouchDB[pulso_twitter] completado!...
Conexión a CouchDB completada.
Conexión a MongoDB completada.
Envio de CouchDB[pulso_twitter] a MongoDB[pulso_twitter] completado!...
Generación de Webscraping de datos de covid de la BBC completada.
Tabla SQLite[covid_webscraping] creada...
Envio de los datos a SQLite[covid_webscraping] completado!...
Conexión a MongoDB completada.
Envio de SQLite[covid_webscraping] a MongoDB[covid_webscraping] completado!...
Generación de datos de Facebook completada.
MySQL: Base de datos covid_facebook creada...
MySQL: Tabla covid_facebook creada...
Envio de los datos a MySQL[covid_facebook] completado!...
Conexión a MongoDB completada.
Envio de MySQL[covid_facebook] a MongoDB[covid_facebook] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727772658847754 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727804644564999 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727828522688512 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727872827117572 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727897850421253 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727922307198982 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727950224539651 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498727972836102144 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498728055770075136 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream encountered HTTP error: 420
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498728398461493254 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498728445466984452 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498728483106607109 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Stream connection closed by Twitter
Generación de datos de Twitter GEO completada.
Conexión a CouchDB completada.
Documento 1498728528770121729 guardado en CouchDB[juegos_twitter]
Envio de los datos a CouchDB[juegos_twitter] completado!...
Conexión a CouchDB completada.
Conexión a MongoDB completada.
Envio de CouchDB[juegos_twitter] a MongoDB[juegos_twitter] completado!...
Generación de datos de Tiktok completada.
Conexión a MongoDBAtlas completada.
Documento 6947711087416495366 guardado en MongoDBAtlas[juegos_tiktok]
Envio de los datos a MongoDBAtlas[juegos_tiktok] completado!...
Conexión a MongoDBAtlas completada.
Conexión a MongoDB completada.
Envio de MongoDBAtlas[juegos_tiktok] a MongoDB[juegos_tiktok] completado!...
Envio de los datos a SQLServer[juegos_dataset1] completado!...
Envio del archivo JuegosDS1.csv a SQLServer[juegos_dataset1] completado!...
Envio de los datos a SQLServer[juegos_dataset2] completado!...
Envio del archivo JuegosDS2.csv a SQLServer[juegos_dataset2] completado!...
Conexión a MongoDB completada.
Envio de SQLServer[juegos_dataset1] a MongoDB[juegos_dataset1] completado!...
Conexión a MongoDB completada.
Envio de SQLServer[juegos_dataset2] a MongoDB[juegos_dataset2] completado!...
'435110554'
'357800000'
'792910554'
'10500000'
Generación de Webscraping de datos de películas taquilleras entre [1980-1990] completada.
'659363944'
'1542283320'
'2201647264'
'200000000'
'422783777'
'640828028'
'1063611805'
'45000000'
'404214720'
'629713583'
'1033928303'
'63000000'
'474544677'
'552538030'
'1027082707'
'115000000'
'306169268'
'511231623'
'817400891'
'75000000'
Generación de Webscraping de datos de películas taquilleras entre [1990-2000] completada.
'760507625'
'2086738578'
'2847246203'
'237000000'
'377845905'
'768185007'
'1146030912'
'94000000'
'423315812'
'642863935'
'1066179747'
'225000000'
'318087620'
'688880551'
'1006968171'
'125000000'
'534858444'
'471115201'
'1005973645'
'185000000'
'309420425'
'651576067'
'960996492'
'310000000'
'342551365'
'604943730'
'947495095'
'94000000'
'292353413'
'649818983'
'942172396'
'150000000'
'380843261'
'559509384'
'940352645'
'94000000'
'302305431'
'632148665'
'934454096'
'250000000'
'441226247'
'487534523'
'928760770'
'150000000'
'315710750'
'581979322'
'897690072'
'93000000'
'290417905'
'606260336'
'896678241'
'150000000'
'336530303'
'558453070'
'894983373'
'258000000'
'196573705'
'690113112'
'886686817'
'90000000'
'262450136'
'617152230'
'879602366'
'100000000'
'380270577'
'488119983'
'868390560'
'113000000'
'402111870'
'434191823'
'836303693'
'200000000'
'407022860'
'418002176'
'825025036'
'140000000'
'322719944'
'490647436'
'813367380'
'160000000'
'249975996'
'547385622'
'797361618'
'130000000'
'166112167'
'625105659'
'791217826'
'200000000'
'317101119'
'473552823'
'790653942'
'185000000'
'374585825'
'415390628'
'789076453'
'200000000'
Generación de Webscraping de datos de películas taquilleras entre [2000-2010] completada.
'858373000'
'1939128328'
'2797501328'
'356000000'
'936662225'
'1132859475'
'2069521700'
'245000000'
'678815482'
'1369544272'
'2048359754'
'356000000'
'652385625'
'1018130819'
'1670516444'
'150000000'
'543638043'
'1119261396'
'1662899439'
'260000000'
'623357910'
'895457605'
'1518815515'
'220000000'
'353007020'
'1162334379'
'1515341399'
'190000000'
'477373578'
'972653355'
'1450026933'
'150000000'
'459005868'
'943803672'
'1402809540'
'250000000'
'700426566'
'647171407'
'1347597973'
'200000000'
'381409310'
'960912355'
'1342321665'
'250000000'
'620181382'
'712517448'
'1332698830'
'317000000'
'417719760'
'892746536'
'1310466296'
'170000000'
'400953009'
'880555091'
'1281508100'
'150000000'
'504481165'
'759953360'
'1264434525'
'160000000'
'608581744'
'636057783'
'1244639527'
'200000000'
'226008385'
'1009996733'
'1236005118'
'250000000'
'409013994'
'805797258'
'1214811252'
'200000000'
'336045770'
'823398892'
'1159444662'
'74000000'
'408084349'
'745253147'
'1153337496'
'250000000'
'335061807'
'813424079'
'1148485886'
'160000000'
'390532085'
'741395911'
'1131927996'
'160000000'
'426829839'
'701633133'
'1128462972'
'152000000'
'352390543'
'771403536'
'1123794079'
'195000000'
'304360277'
'804209222'
'1108569499'
'200000000'
'245439076'
'858614996'
'1104054072'
'210000000'
'448139099'
'633003513'
'1084439099'
'230000000'
'515202542'
'563030047'
'1074232589'
'275000000'
'335451311'
'738968073'
'1074419384'
'55000000'
'434038008'
'639356585'
'1073394593'
'200000000'
'415004880'
'651965935'
'1066970811'
'200000000'
'532177324'
'523880396'
'1056057720'
'265000000'
'355559216'
'695134737'
'1050693953'
'183000000'
'241071802'
'804642000'
'1045713802'
'250000000'
'264624300'
'770175831'
'1034800131'
'80000000'
'486295561'
'542275381'
'1028570942'
'200000000'
'334191110'
'691277106'
'1025468216'
'200000000'
'341268248'
'682852856'
'1024121104'
'150000000'
'303003568'
'714000000'
'1017003568'
'315000000'
'296347721'
'680695762'
'977043483'
'250000000'
'368065385'
'602700620'
'970766005'
'76000000'
'364001123'
'602553806'
'966554929'
'175000000'
'404540171'
'558002774'
'962542945'
'90000000'
'255119788'
'707063077'
'962182865'
'250000000'
'258366855'
'700640658'
'959007513'
'225000000'
'216668042'
'694432685'
'911100727'
'52000000'
'200074609'
'680606910'
'880681519'
'245000000'
'334201140'
'545965784'
'880166924'
'175000000'
'161321843'
'715922939'
'877244782'
'95000000'
'368384330'
'507074301'
'875458631'
'75000000'
'330360194'
'543277334'
'873637528'
'250000000'
'2721100'
'867604339'
'870325439'
'30100000'
'424668047'
'440343699'
'865011746'
'130000000'
'389813101'
'473942950'
'863756051'
'200000000'
'356921711'
'501926308'
'858848019'
'175000000'
'213515506'
'642569645'
'856085151'
'100000000'
'315058289'
'538925622'
'853983911'
'180000000'
'292576195'
'544260772'
'836836967'
'160000000'
'292324737'
'537422917'
'829747654'
'120000000'
'412815408'
'410009114'
'822824522'
'149000000'
'234037575'
'580006426'
'814044001'
'180000000'
'210460015'
'597357873'
'807817888'
'175000000'
'320314960'
'479744747'
'800059707'
'125000000'
'172558876'
'622322566'
'794881442'
'230000000'
'220159104'
'571498294'
'791657398'
'178000000'
'238679850'
'550001118'
'788680968'
'160000000'
'324591735'
'461878749'
'785896609'
'110000000'
Generación de Webscraping de datos de películas taquilleras entre [2010-2020] completada.
'774136947'
'1060000000'
'1863887000'
'200000000'
'268652'
'902111856'
'902111856'
'200000000'
'-'
'822009764'
'822009764'
'59000000'
Generación de Webscraping de datos de películas taquilleras entre [2020-2030] completada.
Tabla SQLite[peliculas_webscraping] creada...
Envio de los datos a SQLite[peliculas_webscraping] completado!...
Documento 2201647264 guardado en SQLite[peliculas_webscraping]
Documento 1063611805 guardado en SQLite[peliculas_webscraping]
Documento 1033928303 guardado en SQLite[peliculas_webscraping]
Documento 1027082707 guardado en SQLite[peliculas_webscraping]
Documento 817400891 guardado en SQLite[peliculas_webscraping]
Envio de los datos a SQLite[peliculas_webscraping] completado!...
Documento 2847246203 guardado en SQLite[peliculas_webscraping]
Documento 1146030912 guardado en SQLite[peliculas_webscraping]
Documento 1066179747 guardado en SQLite[peliculas_webscraping]
Documento 1006968171 guardado en SQLite[peliculas_webscraping]
Documento 1005973645 guardado en SQLite[peliculas_webscraping]
Documento 960996492 guardado en SQLite[peliculas_webscraping]
Documento 947495095 guardado en SQLite[peliculas_webscraping]
Documento 942172396 guardado en SQLite[peliculas_webscraping]
Documento 940352645 guardado en SQLite[peliculas_webscraping]
Documento 934454096 guardado en SQLite[peliculas_webscraping]
Documento 928760770 guardado en SQLite[peliculas_webscraping]
Documento 897690072 guardado en SQLite[peliculas_webscraping]
Documento 896678241 guardado en SQLite[peliculas_webscraping]
Documento 894983373 guardado en SQLite[peliculas_webscraping]
Documento 886686817 guardado en SQLite[peliculas_webscraping]
Documento 879602366 guardado en SQLite[peliculas_webscraping]
Documento 868390560 guardado en SQLite[peliculas_webscraping]
Documento 836303693 guardado en SQLite[peliculas_webscraping]
Documento 825025036 guardado en SQLite[peliculas_webscraping]
Documento 813367380 guardado en SQLite[peliculas_webscraping]
Documento 797361618 guardado en SQLite[peliculas_webscraping]
Documento 791217826 guardado en SQLite[peliculas_webscraping]
Documento 790653942 guardado en SQLite[peliculas_webscraping]
Documento 789076453 guardado en SQLite[peliculas_webscraping]
Envio de los datos a SQLite[peliculas_webscraping] completado!...
Documento 2797501328 guardado en SQLite[peliculas_webscraping]
Documento 2069521700 guardado en SQLite[peliculas_webscraping]
Documento 2048359754 guardado en SQLite[peliculas_webscraping]
Documento 1670516444 guardado en SQLite[peliculas_webscraping]
Documento 1662899439 guardado en SQLite[peliculas_webscraping]
Documento 1518815515 guardado en SQLite[peliculas_webscraping]
Documento 1515341399 guardado en SQLite[peliculas_webscraping]
Documento 1450026933 guardado en SQLite[peliculas_webscraping]
Documento 1402809540 guardado en SQLite[peliculas_webscraping]
Documento 1347597973 guardado en SQLite[peliculas_webscraping]
Documento 1342321665 guardado en SQLite[peliculas_webscraping]
Documento 1332698830 guardado en SQLite[peliculas_webscraping]
Documento 1310466296 guardado en SQLite[peliculas_webscraping]
Documento 1281508100 guardado en SQLite[peliculas_webscraping]
Documento 1264434525 guardado en SQLite[peliculas_webscraping]
Documento 1244639527 guardado en SQLite[peliculas_webscraping]
Documento 1236005118 guardado en SQLite[peliculas_webscraping]
Documento 1214811252 guardado en SQLite[peliculas_webscraping]
Documento 1159444662 guardado en SQLite[peliculas_webscraping]
Documento 1153337496 guardado en SQLite[peliculas_webscraping]
Documento 1148485886 guardado en SQLite[peliculas_webscraping]
Documento 1131927996 guardado en SQLite[peliculas_webscraping]
Documento 1128462972 guardado en SQLite[peliculas_webscraping]
Documento 1123794079 guardado en SQLite[peliculas_webscraping]
Documento 1108569499 guardado en SQLite[peliculas_webscraping]
Documento 1104054072 guardado en SQLite[peliculas_webscraping]
Documento 1084439099 guardado en SQLite[peliculas_webscraping]
Documento 1074232589 guardado en SQLite[peliculas_webscraping]
Documento 1074419384 guardado en SQLite[peliculas_webscraping]
Documento 1073394593 guardado en SQLite[peliculas_webscraping]
Documento 1066970811 guardado en SQLite[peliculas_webscraping]
Documento 1056057720 guardado en SQLite[peliculas_webscraping]
Documento 1050693953 guardado en SQLite[peliculas_webscraping]
Documento 1045713802 guardado en SQLite[peliculas_webscraping]
Documento 1034800131 guardado en SQLite[peliculas_webscraping]
Documento 1028570942 guardado en SQLite[peliculas_webscraping]
Documento 1025468216 guardado en SQLite[peliculas_webscraping]
Documento 1024121104 guardado en SQLite[peliculas_webscraping]
Documento 1017003568 guardado en SQLite[peliculas_webscraping]
Documento 977043483 guardado en SQLite[peliculas_webscraping]
Documento 970766005 guardado en SQLite[peliculas_webscraping]
Documento 966554929 guardado en SQLite[peliculas_webscraping]
Documento 962542945 guardado en SQLite[peliculas_webscraping]
Documento 962182865 guardado en SQLite[peliculas_webscraping]
Documento 959007513 guardado en SQLite[peliculas_webscraping]
Documento 911100727 guardado en SQLite[peliculas_webscraping]
Documento 880681519 guardado en SQLite[peliculas_webscraping]
Documento 880166924 guardado en SQLite[peliculas_webscraping]
Documento 877244782 guardado en SQLite[peliculas_webscraping]
Documento 875458631 guardado en SQLite[peliculas_webscraping]
Documento 873637528 guardado en SQLite[peliculas_webscraping]
Documento 870325439 guardado en SQLite[peliculas_webscraping]
Documento 865011746 guardado en SQLite[peliculas_webscraping]
Documento 863756051 guardado en SQLite[peliculas_webscraping]
Documento 858848019 guardado en SQLite[peliculas_webscraping]
Documento 856085151 guardado en SQLite[peliculas_webscraping]
Documento 853983911 guardado en SQLite[peliculas_webscraping]
Documento 836836967 guardado en SQLite[peliculas_webscraping]
Documento 829747654 guardado en SQLite[peliculas_webscraping]
Documento 822824522 guardado en SQLite[peliculas_webscraping]
Documento 814044001 guardado en SQLite[peliculas_webscraping]
Documento 807817888 guardado en SQLite[peliculas_webscraping]
Documento 800059707 guardado en SQLite[peliculas_webscraping]
Documento 794881442 guardado en SQLite[peliculas_webscraping]
Documento 791657398 guardado en SQLite[peliculas_webscraping]
Documento 788680968 guardado en SQLite[peliculas_webscraping]
Documento 785896609 guardado en SQLite[peliculas_webscraping]
Envio de los datos a SQLite[peliculas_webscraping] completado!...
Documento 1863887000 guardado en SQLite[peliculas_webscraping]
Documento 902111856 guardado en SQLite[peliculas_webscraping]
Documento 822009764 guardado en SQLite[peliculas_webscraping]
Envio de los datos a SQLite[peliculas_webscraping] completado!...
Conexión a MongoDB completada.
Envio de SQLite[peliculas_webscraping] a MongoDB[peliculas_webscraping] completado!...
Envio de los datos a SQLServer[peliculas_dataset3] completado!...
Envio del archivo MoviesDS3.csv a SQLServer[peliculas_dataset3] completado!...
Envio de los datos a SQLServer[peliculas_dataset4] completado!...
Envio del archivo MoviesDS4.csv a SQLServer[peliculas_dataset4] completado!...
Conexión a MongoDB completada.
Envio de SQLServer[peliculas_dataset3] a MongoDB[peliculas_dataset3] completado!...
Conexión a MongoDB completada.
Envio de SQLServer[peliculas_dataset4] a MongoDB[peliculas_dataset4] completado!...
Generación de datos de Facebook completada.
MySQL: Base de datos eventos_facebook creada...
MySQL: Tabla eventos_facebook creada...
Envio de los datos a MySQL[eventos_facebook] completado!...
Conexión a MongoDB completada.
Envio de MySQL[eventos_facebook] a MongoDB[eventos_facebook] completado!...
Generación de datos de Tiktok completada.
Conexión a MongoDBAtlas completada.
Documento 6902238779776470277 guardado en MongoDBAtlas[eventos_tiktok]
Envio de los datos a MongoDBAtlas[eventos_tiktok] completado!...
Conexión a MongoDBAtlas completada.
Conexión a MongoDB completada.
Envio de MongoDBAtlas[eventos_tiktok] a MongoDB[eventos_tiktok] completado!...
Conexión a MongoDB completada.
Envio de MongoDB[peliculas_webscraping] al archivo peliculas_webscraping.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[covid_webscraping] al archivo covid_webscraping.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[juegos_twitter] al archivo juegos_twitter.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[pulso_twitter] al archivo pulso_twitter.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[covid_facebook] al archivo covid_facebook.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[eventos_facebook] al archivo eventos_facebook.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[juegos_tiktok] al archivo juegos_tiktok.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[eventos_tiktok] al archivo eventos_tiktok.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[juegos_dataset1] al archivo juegos_dataset1.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[juegos_dataset2] al archivo juegos_dataset2.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[peliculas_dataset3] al archivo peliculas_dataset3.csv completado!...
Conexión a MongoDB completada.
Envio de MongoDB[peliculas_dataset4] al archivo peliculas_dataset4.csv completado!...
```




