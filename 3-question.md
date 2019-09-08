# Bash Script Challenge

- Why this script does not work and how you can fix this (any solution acceptable)?
```
#!/bin/sh
IFS=$'\n'
set -xe
: "${REACT_APP_API_GATEWAY_URL?Need an environment variable called REACT_APP_API_GATEWAY_URL}"
#replace url variable with environment specific url

FILE_PATHS=$(find /usr/share/nginx/html/static -name 'main.*.chunk.js')
#loop over it
for FILE_PATH in ${FILE_PATHS[@]}
do
    echo "processing file"
    echo FILE_PATH
    sed "s/%%/$/g" $FILE_PATH | envsubst '$REACT_APP_API_GATEWAY_URL' > $FILE_PATH.new
    mv $FILE_PATH.new  $FILE_PATH
done

exec "$@"
```
- (kann nur gefragt werden, wenn die vorherige Frage behandelt worden ist)
  what is the difference between `[@]` vs `[*]` in <u>**bash**</u>?
