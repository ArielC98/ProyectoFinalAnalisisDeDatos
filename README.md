# Proyecto final de Análisis de datos
###Integrantes:
###- Raúl Tenorio
###- Ariel Calderón
###- Mateo Cueva
###- Leonel Molina

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

Después, se oprimen los botones 'Buscar' para comenzar la recolección de datos de cada uno de los temas.

```python
hola
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
