For building(change docker repo name as required)
docker build --compress --no-cache --squash --force-rm -t gaikwadpratik/ubuntu-vscode-golang .

for Debugging use 
docker build --compress --no-cache --squash --force-rm -t gaikwadpratik/ubuntu-vscode-golang . --progress=plain

docker builder prune -a -f