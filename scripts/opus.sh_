#!/bin/bash

# Usage : ./opus.sh $lang_pair
#           lang_pair in es-fr, de-en, fr-ru, en-ru, de-fr, es-it...

# All conditions are there to allow the script to resume or it stopped in case of a sudden stop.

set -e

pair=$1  # input language pair

# folder (container/in which to store) the data 
PARA_PATH=/content/data/para
mkdir -p $PARA_PATH

# opus data source (MultiUN or OpenSubtitles2018 or MultiUN,OpenSubtitles2018) : customize as needed
# SRC=OpenSubtitles2018,MultiUN
SRC=OpenSubtitles,MultiUN

if [ ! -d $PARA_PATH/${pair} ]; then
    mkdir $PARA_PATH/${pair}
else
    echo "dir $PARA_PATH/${pair} already exists"
fi

echo -e "\n"
echo "***Download data and unzip it in $PARA_PATH/$pair ***"

# Updated function to download and unzip data
download_and_unzip_data() {
    base_url="https://object.pouta.csc.fi/OPUS-$2/v2018/moses"
    file_name="${pair}.txt.zip"
    wget -c ${base_url}/${file_name} -P $1/${pair}
    unzip -u $1/${pair}/${file_name} -d $1/${pair}
}

if [ $SRC = "MultiUN,OpenSubtitles" ] || [ $SRC = "OpenSubtitles,MultiUN" ]; then
  if [ $pair != "es-it" ]; then
    # es-fr, de-en, fr-ru, en-ru, de-fr...
    for src in $(echo $SRC | sed -e 's/\,/ /g'); do
      download_and_unzip_data $PARA_PATH $src 
    done
  else
    # es-it...
    for src in OpenSubtitles GlobalVoices EUbookshop; do
      wget -c "https://object.pouta.csc.fi/OPUS-${src}/v2018/moses/es-it.txt.zip" -P $PARA_PATH/${pair}
      unzip -u $PARA_PATH/${pair}/es-it.txt.zip -d $PARA_PATH/${pair}
    done
  fi
elif [ $SRC = "MultiUN" ] || [ $SRC = "OpenSubtitles2018" ]; then
  if [ $pair != "es-it" ]; then
    # es-fr, de-en, fr-ru, en-ru, de-fr...
    download_and_unzip_data $PARA_PATH $SRC 
  else
    # es-it...
    wget -c "https://object.pouta.csc.fi/OPUS-OpenSubtitles/v2018/moses/es-it.txt.zip" -P $PARA_PATH/${pair}
    unzip -u $PARA_PATH/${pair}/es-it.txt.zip -d $PARA_PATH/${pair}
  fi
else
  echo "source error : $SRC is not valid source, choose between MultiUN and OpenSubtitles2018"
  exit
fi

echo -e "\n"
echo "*** Convert to txt***"
for lg in $(echo $pair | sed -e 's/\-/ /g'); do
    if [ ! -f $PARA_PATH/${pair}/$pair.$lg.txt ]; then
        cat $PARA_PATH/${pair}/*.$pair.$lg > $PARA_PATH/${pair}/$pair.$lg.txt
    else
        echo "file $PARA_PATH/${pair}/$pair.$lg.txt already exists"
    fi
done
