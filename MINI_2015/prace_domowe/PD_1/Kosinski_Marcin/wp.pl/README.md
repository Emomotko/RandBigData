# Kodowanie artykułów na portalu wp.pl
Marcin Kosinski  
Marzec 11, 2015  




# Wczytanie pakietu i pobranie listy artykulów


```r
library(rvest)
library(magrittr)
library(stringr)

wp <- html("http://www.wp.pl/")
artykuly <- html_nodes(wp,"div.sArt a") %>%
    html_attrs()


linki_do_artykulow <- lapply( artykuly, function( element ){
    element["href"]
})
    artykuly <- linki_do_artykulow # zeby moc skopiowac kod
 encodings <- lapply( artykuly, function(element){
     
     # for( element in artykuly ){
     landing_page_artykulu <- html(element[1])
     charset <- html_nodes( landing_page_artykulu ,"meta[http-equiv=Content-Type]") %>% 
         html_attr("content") 
          
     if ( length( charset > 0 )){
          encoding <- str_split( string = charset, pattern = "=" )[[1]][2]
          encoding
     }else{ #charset nie jest spojny na wszystkich stronach
          encoding <- html_nodes( landing_page_artykulu , "meta[charset]" ) %>% 
              html_attrs( )   
          encoding[[1]]
     }
     
     
     
 })


encodings <- tolower(unlist(encodings))
```

# Jakie sa kodowania dla artykulów ze strony glównej wp.pl


```r
table(encodings)
```

```
encodings
iso-8859-2      utf-8 
        27         38 
```


# Dla ilu artykulów kodowanie bylo wprowadzone inaczej niz poprzez `meta http-equiv`



```r
table(names(encodings))
```

```

        charset 
     55      10 
```

```r
# puste to standardowy sposób
# o nazwie "charset" to archaizm
```


