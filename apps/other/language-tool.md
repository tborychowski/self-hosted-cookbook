# LanguageTool
Multilingual grammar, style, and spell checker (similar to Grammarly).

<br>

- [Official Site](https://languagetool.org)
- [Github repo](https://github.com/Erikvl87/docker-languagetool)
- [Docker Hub](https://hub.docker.com/r/erikvl87/languagetool)

## Optional n-gram data
LanguageTool can make use of large n-gram data sets to detect errors with words that are often confused, like `their` and `there`.

1. Download n-gram data: https://languagetool.org/download/ngram-data/
2. Uncomment n-gram volume and env variable below

## docker-compose.yml
```yml
---
version: "3"
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
    # volumes:
    #     - ./data:/ngrams
```

And then, in a browser extension:
- in `LanguageTool API server URL:` select `Other server`
- enter: `http://<SERVER IP>:8010/v2`
