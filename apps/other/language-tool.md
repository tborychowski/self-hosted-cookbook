# LanguageTool
Multilingual grammar, style, and spell checker (similar to Grammarly).

<br>

- [Official Site](https://languagetool.org)
- [Github repo](https://github.com/Erikvl87/docker-languagetool)
- [Docker Hub](https://hub.docker.com/r/erikvl87/languagetool)

## Optional n-gram data
LanguageTool can make use of large n-gram data sets to detect errors with words that are often confused, like `their` and `there`.

1. Download & unzip n-gram data for your language: https://languagetool.org/download/ngram-data/

   ```sh
   wget https://languagetool.org/download/ngram-data/ngrams-en-20150817.zip
   unzip ngrams-en-20150817.zip
   ```

2. Uncomment n-gram volume and env variable below

## docker-compose.yml
```yml
services:
  languagetool:
    image: erikvl87/languagetool
    container_name: languagetool
    restart: unless-stopped
    ports:
        - 8010:8010  # Using default port from the image
    environment:
        # - langtool_languageModel=/ngrams  # OPTIONAL: Using ngrams data
        - Java_Xms=512m  # OPTIONAL: Setting a minimal Java heap size of 512 mib
        - Java_Xmx=1g  # OPTIONAL: Setting a maximum Java heap size of 1 Gib
    volumes:
        - ./ngrams:/ngrams
```

And then, in a browser extension:
- in `LanguageTool API server URL:` select `Other server`
- enter: `http://<SERVER IP>:8010/v2`



## Alternative docker-compose.yml
This is using [libregrammar](https://github.com/TiagoSantos81/libregrammar) version, which (allegedly) provides the premium features for free.

- [Github repo for libregrammar](https://github.com/TiagoSantos81/libregrammar)
- [Github repo for the docker image](https://github.com/py-crash/docker-libregrammar)


```yml
services:
  languagetool:
    image: registry.gitlab.com/py_crash/docker-libregrammar
    container_name: languagetool
    restart: unless-stopped
    environment:
      - langtool_languageModel=/ngrams  # OPTIONAL: Using ngrams data
      - Java_Xms=512m  # OPTIONAL: Setting a minimal Java heap size of 512 mib
      - Java_Xmx=1g  # OPTIONAL: Setting a maximum Java heap size of 1 Gib
    ports:
      - "8081:8081"
    volumes:
      - ./ngrams:/ngrams
```
