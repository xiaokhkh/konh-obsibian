


flamebearer 

node --prof xxx.js 

node --prof-process isolate*.log | process.txt

node --prof-process --preprocess -j isolate*.log | flamebearer


https://github.com/clinicjs/node-clinic 分析