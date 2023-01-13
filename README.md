# To get IAGS input (windows)

The blocks file used by IAGS can be generated by DRIMM-synteny.

[中文版本](./README_zh.md)

## 1. Generate the input file for DRIMM-synteny (processOrthofinder.py)

```python
dir = './example'
sp = ['Brachy','Maize','Rice','Sorghum','Telongatum']
sp_ratio = [2,4,2,2,2]
```

+ dir: Gff files of all species (gff file format is the same as [MCScanX](https://github.com/wyp1125/MCScanx) input) and Orthogroups.tsv ([OrthoFinder](https://github.com/davidemms/OrthoFinder) output file) files path.
+ sp: Species names
+ sp_ratio: Target copy number for species, e.g. one for no WGD event and two for one WGD event

The gff files need to be named after the species name (for example: Brachy.gff), the naming method of the first column of chromosomes in the gff file is specie_N, and the chromosome number starts from 1 (for example: Brachy_1). In addition, the species name in the first line of the Orthogroups.tsv file needs to correspond to the file name (ie, the species name) of gff.

processOrthofinder generates three files, namely:

+ .all.sequence: The complete species homologous gene sequence. Each row represents a chromosome, and genes are represented by homologous gene ID
+ .all.sequence.genename: The format is the same as .all.sequence, but the genes are represented by the gene name
+ .sequence: Filtered species homologous gene sequence. Filter the homologous genes exceeding the target copy number in .all.sequence according to the species target copy number, represented by the homologous gene ID

Then all the .sequence files are merged to get the drimm.sequence file as the input of DRIMM-synteny.

## 2.Running  DRIMM-synteny (drimm.exe)

Since the version of dotnet may be different in each PC, we recommend recompiling to generate the new .exe file.

+ Using `dotnet publish -c Release -r win-x64` in  ./drimm to obtain drimm.exe (in ./drimm/drimm/bin/Release/netcoreapp3.1/win-x64)

Running drimm.exe

<div align="center">
<img src="https://i.328888.xyz/2023/01/13/wUDip.png">
</div>


The input of DRIMM-synteny:
+ The path of drimm.sequence
+ The output directory
+ cycleLengthThreshold controls the continuity of synteny blocks (default parameter is 20)
+ dustThreshold controls the upper limit of gene family. The gene family will be filtered when homologous genes exceeding dustThreshold. For above example, target copy numbers are 2,4,2,2,2 in each species, then the dustThreshold is 13 (2+4+2+2+2+1)

The output of DRIMM-synteny
+ synteny.txt
```
0:1 2147483647 -1 
1:1 8997 
2:1 -2147482956 -2147482957 -2147482958 -2147482959 -2147482960 -2147482961 -2147482962 -2147482963 -2147482964 -2147482965 -2147482966 -2147482967 -2147482968 -2147482969 -2147482970 -2147482971 -2147482972 -2147482973 -2147482974 -2147482975 -2147482976 
3:1 13651 10527 2147481608 
4:1 10498 6507 
5:1 7035 2147482195 3426 -2147482913 -2147482912 -2147482911 -2147482910 -2147482909 -2147482908 -2147482907 -2147482906 -2147482905 -2147482904 -2147482903 -2147482902 -2147482901 -2147482900 -2147482899 -2147482898 -2147482897 -2147482896 -2147482895 -2147482894 -2147482893 
6:1 10464 
7:1 2147480311 
8:1 8926 8925 
9:1 6472 2147482102 992 
10:1 2147481278 13638 2147479645 128 
11:1 8808 13137 
12:1 2147481394 -2147483438 -2147483437 -2147483436 -2147483435 -2147483434 -2147483433 -2147483432 -2147483431 -2147483430 -2147483429 -2147483428 -2147483427 -2147483426 -2147483425 -2147483424 -2147483423 -2147483422 -2147483421 -2147483420 -2147483419 -2147483418 2147483493 10949 
13:1 2147478991 

#The first column is the ID of the block
#The second column (after the colon) is the number of block occurrences in all species
#The third column and later are the IDs of the homologous genes
```
+ blocks.txt
```
-261 -434 -774 -1204 -1358 -1227 -1393 479 -1227 -1393 -1031 -1159 676 -971 -561 921 911 782 -918 1317 -827 -518 -897 1324 -1381 1311 -1377 -1310 -851 300 -860 -1287 -1438 -1430 1421 -843 -430 381 668 421 321 539 415 -472 -646 -475 -1001 -1303 -1072 -796 -1075 -1081 -1271 1085 -1091 1272 1051 -1282 -1379 1291 -1150 629 -1173 -934 -1335 -958 -923 1220 -1066 -1062 -768 1206 -607 -1054 -1230 -1048 755 -1044 -729 -1007 -658 -998 -815 -1035 -605 -638 -1023 -1012 1056 -713 -1196 -1176 686 -1100 -662 -981 -368 -1200 -285 -1125 -777 -1170 -641 -865 -1223 -848 -687 -951 -681 -950 -943 -714 -588 -698 -966 -647 -957 -810 -576 -899 880 -731 885 -580 -920 -914 -913 -912 -570 764 -1437 -783 916 -776 -1437 -783 916 590 1258 -878 -832 -792 -230 -348 -890 1225 -926 -1332 963 1328 936 -1323 -975 574 423 -245 -45 226 44 
-716 937 -571 -942 -1312 -944 -602 -947 -675 -949 -876 1373 905 -1337 -856 -665 622 603 -855 337 -873 -594 -846 -864 -733 -730 -1380 -1369 -869 -601 -215 733 -850 -980 769 -1256 -779 575 -1380 -1369 -450 -840 -1367 -1126 -797 -1128 1130 459 -1405 -1117 1187 706 -1195 717 -584 -398 1253 -586 1342 -1156 -591 696 -1408 1357 -1314 1039 1359 -1268 316 320 -417 -313 473 -653 403 -645 1422 -990 1030 -1302 -1006 -1040 999 1289 1397 -1288 -1078 -1286 395 965 454 -1089 -745 -1090 -742 -1093 790 -1095 -1067 -1045 -572 784 941 -781 -1053 -789 -1058 -462 -1063 1232 1064 1350 -1402 -1285 -1284 -1065 -762 -1061 -1060 -766 -1059 -1057 -771 -772 1445 -1415 -1392 -1055 -1280 1392 1415 788 -788 -1445 -1047 -1277 -436 597 -1276 -734 -739 -1092 -482 -480 1088 -748 -750 -1087 -1273 -1067 1350 -1402 -255 925 -16 1095 -790 1093 742 1090 745 1089 -454 -965 -1286 380 1078 1288 -1397 -1289 -999 1040 1006 1302 1030 990 -1422 645 199 1388 290 653 17 
-1024 -1304 1004 1203 -239 1041 -997 496 -812 -995 -994 -993 -821 1298 -1297 -991 988 -1296 -986 -499 -985 461 749 1008 -723 -1009 -1011 829 583 1187 706 -1195 717 -584 -398 1253 -586 1342 204 474 1422 696 -1408 1357 -1314 1039 1359 -1268 -643 -655 1038 1290 1037 -650 582 1099 -630 1022 996 806 -540 820 -1019 -625 -1388 -1017 1344 1016 1216 956 1321 1013 -617 1020 287 1076 -286 1129 1174 1189 -1245 -1363 -1166 -1164 801 -1161 -1397 -1157 -722 -1154 1348 1248 -1175 -1178 -715 1172 -716 937 571 -942 -1312 -944 675 947 -949 -876 200 873 440 855 622 603 665 856 1337 -905 -1373 431 -594 -846 840 733 -850 -864 -601 -730 -1380 -1369 -869 455 -575 779 1256 -769 980 -1367 -1126 -797 -1128 537 1130 -597 389 -1276 -734 -739 -1092 480 482 1088 -748 -750 -1087 -1273 -925 -1183 874 1235 -688 -674 -1149 -680 394 274 
-1250 1116 1327 -1115 -791 -1113 -1266 -1112 -679 1111 1265 -22 1217 -305 1231 1106 -1105 -1262 1005 719 1073 847 1050 1046 1251 -1101 -1375 -1122 -1307 -1146 1259 -697 -1145 -657 -644 1182 628 627 -1257 -1141 -1139 -564 -1137 807 -566 1136 1267 -1132 1293 -535 798 -419 844 -830 -754 -1103 -1107 -743 -741 1110 -1120 -1179 -1441 -1152 868 866 894 1281 297 -21 
-21 -1357 1408 770 1165 -894 -866 1441 868 1152 1441 1179 -1120 -1110 741 743 1107 505 -409 844 -830 -754 -1103 505 -409 577 311 533 577 -535 798 -1293 1132 -1267 -1136 807 -566 1137 -608 -1139 -564 -608 1139 1141 1257 -627 -628 -1182 644 -697 -1145 -657 -1259 1146 1307 1122 1375 1101 -1251 -1046 -1050 467 -1020 617 -1013 -1321 -956 -1216 -1016 -1344 1017 1388 625 1019 -820 540 -806 -996 -203 -1022 630 -1099 1099 -1037 -1290 -1038 655 643 -1172 715 1178 1175 -1248 -1348 1154 -722 1157 1189 -631 -611 -862 1032 -689 606 -700 -1438 -1430 1421 490 843 -1421 1430 1438 1287 860 445 851 1310 1377 -1311 1381 -1324 897 238 921 911 782 -918 1317 971 -676 1159 1031 1393 1227 1358 1204 -1393 479 1358 1204 774 434 261 
-423 -574 975 1323 -936 -1328 -963 1332 926 -1225 890 -842 -590 418 -740 1437 792 832 878 -1258 -590 -916 783 1437 -764 570 912 -914 -913 920 880 -731 885 193 899 576 -588 698 -413 -966 -647 -957 -810 714 -848 950 681 951 -943 579 1223 865 -641 1170 777 1125 593 -1200 981 662 1100 -686 1176 1196 713 -1056 1012 1023 -301 -605 -638 729 -658 -998 815 -1035 -1007 1044 -755 1048 1230 1054 607 -1206 768 1062 1066 -1220 923 958 1335 934 1173 -629 1150 -1291 1379 1282 -1051 -1272 1091 -1085 1271 1081 1075 796 1072 1303 1001 -415 -539 -321 -421 -668 -381 430 
-933 -932 721 -704 -702 -940 -1319 -954 -695 -694 955 -1333 -976 -616 -974 -623 849 1162 1322 1079 973 -528 -875 1180 -621 -1353 -618 -969 893 -527 -1279 928 838 364 -526 854 523 -420 -189 -442 545 1398 -959 -362 -1398 1080 1077 1002 -339 -929 1094 898 1238 -1427 -1331 726 -895 1097 -517 -886 -1345 1383 642 889 881 879 1131 -673 -887 -209 900 809 -902 1123 922 -1346 -919 -917 -1340 -220 -1297 -991 988 -1296 -986 -1011 985 1009 723 -812 -361 -583 -829 461 749 1008 997 -234 994 995 -821 1298 867 549 463 786 232 -699 -904 243 -1041 492 -632 -1203 -1004 1304 1024 800 516 898 -426 282 1240 
93 -186 -393 -562 -1250 1116 1327 -1115 -791 -1113 -1266 -1112 -679 1111 1265 1217 560 1231 1106 -1105 -1262 1005 719 1073 847 1076 514 1129 318 1174 1161 -801 1164 1166 1363 1245 -841 -559 -336 -1363 -198 -811 932 933 721 -704 695 954 1319 -955 694 -702 -940 -1333 -976 -616 -974 -623 849 1162 1322 1079 973 969 -149 -1180 875 893 838 -1279 -527 -1279 928 526 456 319 -382 -786 -463 -549 -867 1340 917 919 1346 -922 1123 900 809 -902 511 1131 -673 -887 509 -879 -881 -889 -642 -1383 1345 -263 -1383 1345 886 347 258 345 1097 -345 895 -726 1331 1427 -1238 633 1034 443 344 727 -968 1329 -972 1010 

#Synteny blocks for species. The sign indicates direction.
```
## 3.Generate IAGS input (processDrimm.py)
```python
block_file = './processDRIMM/example/drimm/blocks.txt'
synteny_file = './processDRIMM/example/drimm/synteny.txt'
outdir = './processDRIMM/example/drimm/'
chr_number = [5,10,12,10,7]
sp_list = ['Brachy','Maize','Rice','Sorghum','Telongatum']
target_rate = '2:4:2:2:2'
```

+ block_file: The path of block.txt
+ synteny_file: The path of synteny.txt
+ outdir: The output directory(Part of the files generated by processOrthofinder would be used, so it should be consistent with the output path of processOrthofinder)
+ chr_number: The number of chromosomes in each species
+ sp_list: All species name
+ target_rate: The target copy number of each species

Firstly, processDrimm splits the blocks file to obtain the .block file of each species, and restores the synteny block sequence in the .block files to the homologous gene ID sequence. Since the gene that does not match the copy number is filtered in the previous step, the Longest Common Subsequence(LCS) algorithm is used to match it with the complete homologous gene ID sequence of the species. Then the homologous gene ID sequence and genename sequence of each synteny block in the original species are recovered for downstream biological analysis. Finally, the synteny blocks that do not meet the expected copy number in the .block files of each species are filtered to obtain the .final.block files for IAGS input. 

+ LCS schematic
  ```
  1720:1 24 1256
  1640:1 33 666 1230 720 140
  666:1 557 886
  
  # Correspondence between synteny blocks ID and homologous genes in blocks.txt
  ```
  
  ![s1L27.png](https://i.328888.xyz/2023/01/13/wU45L.png)

  <div align=center><b>Fig. 1</b> Construction of complete homologous gene sequences in synteny blocks.</div>

  ```
  1720:1 24 288 1256
  1640:1 366 666 1230 720 2348 140
  666:1 557 567 886
  
  # After LCS matching, The complete homologous genes ID of the synteny blocks in species A
  ----------------------------------------------------
  1720:1 a1 a2 a3
  1640:1 a4 a5 a6 a7 a8 a9
  666:1 a10 a11 a12
  
  # After LCS matching, the complete homologous gene name of the synteny blocks in species A
  ```
  
  ```
  1720:1 24 357 1256
  1640:1 666 1230 543 720 1440 140
  666:1 557 886
  
  # After LCS matching, The complete homologous genes ID of the synteny blocks in species B
  ----------------------------------------------------
  1720:1 b1 b2 b3
  1640:1 b4 b5 b6 b7 b8 b9
  666:1 b11 b12
  
  # After LCS matching, the complete homologous gene name of the synteny blocks in species B
  ```

The final output has two folders, drimmBlocks and finalBlocks, and the file formats in the two folders are same. The difference is that the .block files in drimmBlocks are unfiltered block sequences. The other two .synteny and .synteny.genename files are the synteny block information corresponding to its .block file. The .final.block files in finalBlocks are the blocks matched the expected copy number of the species after filtering, and the other two .synteny and .synteny.genename files are the synteny block information corresponding to the .final.block file. The .final.block files are used as the input file for IAGS.

Example of file format:

+ Brachy.block

```
s -670 -1019 -1360 -1025 -1369 -632 -1344 360 -571 -736 1351 -1337 -1349 -852 -1368 -1182 739 -1629 -732 -1590 -768 -721 1487 847 -771 252 -777 -817 -1592 -476 -498 -1239 -1220 -1378 -1388 1363 -1622 -752 -1649 1418 1580 -1062 -1635 -1481 -1358 620 -1073 -583 -435 315 -354 881 -592 -343 -975 -450 849 -1560 -988 -1561 -999 -613 -1390 -957 1384 1679 -993 -1192 -904 -1482 887 -1485 -1343 1309 -1281 1147 909 1304 1118 1218 -1604 -1376 -1288 1376 1604 1468 -1289 -1291 -1058 542 -1063 972 -1018 700 -471 692 -324 -687 1069 -387 -81 451 1021 1495 1037 291 -1088 1401 -1080 -1467 1285 -1479 1554 -1462 -1081 1366 1240 1562 -622 -1091 1272 913 -1662 1631 -1540 -1339 -931 718 -1341 -679 -1009 286 -1539 -1483 -1565 1341 -882 -1398 -1488 -1526 -1396 -1692 -1162 -889 645 -1512 -1448 1492 -614 -1319 -944 -1447 -651 -646 1559 -639 -1321 -1515 -1098 -950 -625 -953 -1505 -954 -1405 1406 -963 1408 843 -1564 -1409 1310 1348 -1535 -973 -1538 -1508 -1546 -968 -967 1544 1542 -964 -595 -960 -1541 -958 -603 -1383 -1543 1392 -606 -1359 -948 -947 -946 -945 -100 1094 892 -977 77 995 704 -1360 626 1093 -1004 -1369 -1003 -691 829 695 1351 -1337 -1349 -709 -1368 -1182 -91 -438 448 382 994 -682 1590 -989 1629 -986 -985 1000 -984 1487 -983 1289 -1468 -1604 -1376 -1288 -1256 -1304 -909 -1147 1281 -1309 1343 1485 -538 1482 915 518 -1679 -1384 600 1390 -1153 -902 1561 -901 1560 -900 766 1154 -896 -1316 -11 996 894 -516 714 -1333 -1312 -1216 730 -1335 908 -497 1466 -1529 1166 -941 1442 -939 -938 -1506 -1495 -936 1443 -1504 -1501 -1446 -930 -1502 -1243 -1510 -1241 401 -1524 1024 -1522 978 1521 916 2 
s -912 1164 -942 -1010 -1227 1090 -828 1450 1020 684 -1074 1194 -1416 -1519 -1518 1480 -535 -1206 -1255 -1231 -1417 -1395 -550 -1039 -1221 -1035 -1030 827 924 547 -1426 -543 -541 407 -540 -1425 1305 1424 -536 -492 -1138 -1423 1300 -1422 1299 1016 1174 -556 -1428 -410 -587 1705 -1012 -1138 -587 1705 -1012 -524 1038 1168 1633 1632 974 -1067 1551 -1056 -1630 -1052 -320 -1146 1523 876 -1628 -1045 -1099 -509 -1429 -1626 -1042 -1625 -1431 -1624 -1445 -1444 -1672 -1620 -744 -1619 -734 -729 -1641 -725 -1644 -1566 -724 -1643 -746 1557 747 -767 1652 -765 -1648 -722 1514 -1449 -1665 -1591 -1664 -1150 -1663 -1172 -1435 1308 -1647 1434 -1650 -1407 -1653 -1618 -842 -1180 -841 840 1414 -1402 1528 -1393 -545 -831 -890 -1424 -1305 1425 -830 878 228 -1257 -534 530 1214 1395 1417 -866 -1114 -1480 1518 1519 1416 -531 -864 -1423 1300 -1422 1299 -584 -857 -1428 1022 1585 914 -1426 1617 970 1028 1071 1077 -1582 1096 578 570 336 428 -794 -1112 -1705 -787 1017 -562 -1040 -561 1633 1041 1632 1084 -780 1293 1066 1551 1054 -1630 779 -778 -1143 -1146 1523 -783 -1628 838 -1429 -1626 -797 -1625 -1431 -1624 -1445 -1444 -1672 -1620 -798 792 -1619 -821 1117 -820 -1641 -818 -1644 -1566 776 -1643 -785 1557 891 737 1652 764 -1648 -816 1514 -1449 -1665 -1591 -1664 -1150 -1663 -808 -1435 1308 -1647 1434 -1650 -1407 -1653 -1618 965 1013 1393 -1528 1402 -1414 -802 -1061 1582 1078 -1382 1617 -801 
s 181 -803 920 -671 520 -811 880 -660 -473 1339 1540 -1631 1662 -1179 -1272 935 -1562 -1032 1462 -1554 1479 -1285 1467 -1401 623 1043 -1322 -1539 -1483 -1565 -774 -1398 -1488 -1526 -1396 -1692 -1203 -1512 -1448 1492 -654 -1319 481 -1447 1321 567 1170 -1559 1155 -1515 617 420 -1160 -580 44 329 1228 489 1244 1205 -599 1585 1085 851 966 -1097 758 -1235 -806 -1608 -860 -861 1705 1083 1007 -862 246 616 -1489 1611 987 529 -436 -865 -480 -636 893 -805 -1119 -371 1450 -1264 -1667 272 -1282 1316 -789 -1207 1210 -836 1246 1187 1667 491 -1333 -1312 314 657 1466 -1529 -664 -845 1442 824 1524 1026 1241 519 -675 1510 1243 1502 1443 -1504 -1501 -1446 1059 1506 -823 -1522 917 1521 756 1001 699 710 -1123 -693 -621 -1453 -602 -1454 1345 1456 -711 -1457 1459 680 751 -404 -1460 -751 -643 1411 -1301 -1248 -1615 1575 1463 -1609 -1205 -1244 -604 1413 1520 1095 -1577 -1579 -1581 -733 1594 -1302 1572 -726 1589 -1375 -1656 -731 1470 -1471 1472 -411 -668 1031 863 -489 1121 -1269 -1185 -1181 1433 -1101 -1639 1377 551 1332 1412 -673 -1347 527 -1356 1614 813 -1651 -738 -665 1440 -727 -1588 839 1645 -762 -1578 898 1587 1473 -1573 1593 1612 -598 1576 -757 -648 -1602 -749 1598 -723 716 -1600 -719 1051 -1354 -720 1584 952 -1505 759 -609 -1405 772 -1541 815 -1383 -1543 1392 -846 -1359 859 1027 -1408 -844 -1406 -835 1409 1564 -832 728 -1542 -1544 -872 969 1546 1508 1538 -870 1535 -1348 -1310 959 761 479 -130 
s 804 -1592 1189 393 -1378 -1388 1363 -1622 754 -775 -1649 1418 1580 -991 -1635 -1481 -1358 1011 1605 -800 781 1627 -1278 -1159 -807 -812 -1141 475 -514 -350 1371 357 669 -263 -416 853 -1667 437 -1207 1210 -466 389 546 -1072 -1605 784 -826 1627 418 -434 -1240 -1366 1278 -1627 826 662 -769 703 507 -1173 -383 -485 -1124 624 619 -513 998 610 446 -356 392 444 1065 -502 -511 -1044 1251 1055 -1047 -1060 -422 1313 -1374 -1634 1372 1509 -1427 1295 -1595 1661 1511 1438 1637 1029 -1700 -1693 1513 1365 -449 686 -328 -1187 -1246 1103 -1218 -1118 -921 1123 715 -1453 -925 -1454 1345 1456 -711 -1457 1459 -577 1460 -572 956 1411 -1301 -1248 -1615 753 1575 1463 -1609 850 1413 1520 -933 -1577 -1579 -1581 -940 1594 -1302 1572 -911 1589 -1375 -1656 -884 1470 -1471 1472 1107 -1280 -1163 1075 -895 -899 1008 -368 427 303 -394 -442 -1371 -1060 -400 -649 1313 -1374 -1634 1372 1509 -1427 1295 -1595 1661 1294 1693 1700 1260 -1637 -1438 -1511 1184 -1329 1247 1513 1365 -1191 219 
s -219 589 1049 1129 992 982 745 310 -628 -949 -403 681 1228 448 382 -1181 1433 742 -1639 1377 1048 1332 1412 1057 -1347 712 -1356 926 -1614 -888 -1651 928 1440 717 -1588 822 1645 -885 -1578 -883 740 1587 1473 -1573 1593 1612 -1158 1576 -934 -1602 1079 1598 -927 559 1036 -1600 1064 683 -923 -814 -697 533 -1354 886 1584 1046 -1611 1489 -1023 -1076 -1235 1092 -1608 760 867 -413 591 -1086 -825 501 440 

# Synteny block information for species: plus and minus sign indicate orientation, s indicate linear chromosomes
```
+ Brachy.final.block
```
s -1590 -1592 -1622 -1649 -1635 -1561 1554 1562 -1662 1631 -1540 -1565 -1692 -1512 -1505 -1564 -1535 -1546 1544 1542 -1541 1590 1561 -1506 -1502 -1510 -1524 -1522 1521 
s -1519 -1518 1632 1551 -1630 1523 -1628 -1626 -1625 -1624 -1619 -1643 1557 1652 -1648 1514 -1665 -1664 -1663 -1653 -1618 1528 1518 1519 -1582 1632 1551 -1630 1523 -1628 -1626 -1625 -1624 -1619 -1643 1557 1652 -1648 1514 -1665 -1664 -1663 -1653 -1618 -1528 1582 
s 1540 -1631 1662 -1562 -1554 -1565 -1692 -1512 1524 1510 1502 1506 -1522 1521 1520 -1577 -1579 1594 1589 -1656 -1651 -1588 1645 -1578 1587 -1573 1593 1612 -1602 1598 -1600 -1505 -1541 1564 -1542 -1544 1546 1535 
s -1592 -1622 -1649 -1635 -1634 1509 1511 -1700 1520 -1577 -1579 1594 1589 -1656 -1634 1509 1700 -1511 
s -1651 -1588 1645 -1578 1587 -1573 1593 1612 -1602 1598 -1600 

# Synteny block information for species: plus and minus sign indicate orientation, s indicate linear chromosomes
# Data in .final.block is the blocks that are matched the expected copy number of the species after filtering, which can be used as IAGS input. The format is the same as the .block in the drimmBlocks folder.
 ```

+ Brachy.synteny
```
1344:1:chr_1:- 8531 4797 8530 8529 8528 8527 4796 4795 3525 3525 8526 
360:1:chr_1:+ 8535 8536 8537 16521 3528 3528 1501 8538 8539 2063 
571:1:chr_1:- 8550 8549 8548 1502 8547 8546 8545 16522 4800 233 1502 4803 8544 4802 16524 16524 8543 4801 16523 2064 2064 8542 8541 8540 4800 
736:1:chr_1:- 4821 8583 8582 8581 8580 16522 8579 4820 16529 8578 4819 8577 2743 216 8576 8575 19383 654 8574 8573 8572 4818 18189 3535 8571 3534 16528 7 2068 8570 2067 2067 8569 19382 8568 653 8567 4817 8566 8565 3533 8564 652 16527 8563 3532 4816 4815 770 8562 8561 2066 430 430 4814 4813 8560 4812 75 2742 2065 4811 8559 8558 4810 4809 8557 1503 8556 18188 8555 8554 16526 4808 16525 3531 8553 18187 188 3530 4807 18187 4806 4805 3529 8552 8551 4804 
1351:1:chr_1:+ 1130 19384 4822 3536 1504 4823 4824 8584 3537 3538 8585 4825 1131 3539 1132 
1351:2:chr_1:+ 3539 10097 10098 10099 1131 5533 58 5533 58 5534 1669 1513 5535 1504 5536 3779 5537 66 2744 1132 
1337:1:chr_1:- 4831 3542 905 905 3542 772 552 4830 3541 771 2747 8590 4829 2746 8589 655 2069 8588 3540 8587 4828 4827 16530 18191 8586 1133 904 904 4826 

# In the drimmBlocks folder, the block information corresponding to Brachy.block (in the form of homologous gene ID)
# The first column is the ID of the block
# The second column (after the first colon) is the ordinal number of the block in the species (counting from left to right, top to bottom)
# The third column (after the second colon) is the chromosome number where the block is located
# The fourth column (after the third colon) is the direction of the block, the "+" sign represents the forward direction, and the "-" sign represents the reverse direction
# The fifth column (after the first space) and later is the ID of the homologous genes of the block in the complete species after LCS matching. The order of gene ID is based on the direction of the block
```
+ Brachy.synteny.genename
```
1344:1:chr_1:- BrachyLOC100824538 BrachyLOC100824232 BrachyLOC100846891 BrachyLOC100823620 BrachyLOC100823311 BrachyLOC100837357 BrachyLOC100835193 BrachyLOC100823000 BrachyLOC100831928 BrachyLOC100827351 BrachyLOC100825798 
360:1:chr_1:+ BrachyLOC100846788 BrachyLOC100827931 BrachyLOC100831215 BrachyLOC100828236 BrachyLOC100829136 BrachyLOC100829432 BrachyLOC100829742 BrachyLOC100836227 BrachyLOC100841009 BrachyLOC100837153 
571:1:chr_1:- BrachyLOC100820982 BrachyLOC100834963 BrachyLOC100842013 BrachyLOC100834355 BrachyLOC100834050 BrachyLOC100828672 BrachyLOC104581938 BrachyLOC100833739 BrachyLOC100826519 BrachyLOC104581507 BrachyLOC100833121 BrachyLOC100832621 BrachyLOC100832810 BrachyLOC100825386 BrachyLOC104581937 BrachyLOC112269979 BrachyLOC100822205 BrachyLOC100831892 BrachyLOC104581499 BrachyLOC100831283 BrachyLOC100830666 BrachyLOC100823441 BrachyLOC100825079 BrachyLOC100829071 BrachyLOC100844150 
736:1:chr_1:- BrachyLOC100827865 BrachyLOC100827045 BrachyLOC100825185 BrachyLOC100824575 BrachyLOC100844410 BrachyLOC100843811 BrachyLOC100843504 BrachyLOC100842627 BrachyLOC100843205 BrachyLOC100837460 BrachyLOC100842900 BrachyLOC100830805 BrachyLOC100821902 BrachyLOC104581611 BrachyLOC100842594 BrachyLOC100832937 BrachyLOC112271776 BrachyLOC100825894 BrachyLOC100823743 BrachyLOC100846793 BrachyLOC100843749 BrachyLOC100842113 BrachyLOC100841205 BrachyLOC100837763 BrachyLOC100836229 BrachyLOC100834999 BrachyLOC100834085 BrachyLOC104581598 BrachyLOC112269749 BrachyLOC100841984 BrachyLOC106866297 BrachyLOC100830601 BrachyLOC100841680 BrachyLOC100841075 BrachyLOC100840771 BrachyLOC104581942 BrachyLOC100842729 BrachyLOC100841204 BrachyLOC100831816 BrachyLOC100840160 BrachyLOC100834175 BrachyLOC100837359 BrachyLOC100839861 BrachyLOC100839556 BrachyLOC100828470 BrachyLOC100825274 BrachyLOC100839250 BrachyLOC100838943 BrachyLOC100845773 BrachyLOC112269747 BrachyLOC100827652 BrachyLOC100838335 BrachyLOC100837855 BrachyLOC104581552 BrachyLOC100838034 BrachyLOC100839676 BrachyLOC100837727 BrachyLOC100832313 BrachyLOC100825082 BrachyLOC100837426 BrachyLOC100835084 BrachyLOC100822721 BrachyLOC100836810 BrachyLOC100842934 BrachyLOC100836500 BrachyLOC100839070 BrachyLOC104581531 BrachyLOC100832019 BrachyLOC100834173 BrachyLOC112269745 BrachyLOC100829272 BrachyLOC104581526 BrachyLOC104581525 BrachyLOC104581522 BrachyLOC100825698 BrachyLOC100835887 BrachyLOC100823038 BrachyLOC104581940 BrachyLOC100821479 BrachyLOC100845467 BrachyLOC100839494 BrachyLOC104581939 BrachyLOC100837659 BrachyLOC100835614 BrachyLOC100832933 BrachyLOC100830599 BrachyLOC100829370 BrachyLOC100826009 
1351:1:chr_1:+ BrachyLOC100845939 BrachyLOC100846242 BrachyLOC100846546 BrachyLOC100843947 BrachyLOC100821048 BrachyLOC100821347 BrachyLOC100823140 BrachyLOC100824055 BrachyLOC100826835 BrachyLOC100821650 BrachyLOC100821968 BrachyLOC100831931 BrachyLOC100822276 BrachyLOC104581638 BrachyLOC100822586 
1351:2:chr_1:+ BrachyLOC100830855 BrachyLOC100829026 BrachyLOC100829325 BrachyLOC100829935 BrachyLOC100838252 BrachyLOC100838549 BrachyLOC104581686 BrachyLOC100839663 BrachyLOC100840278 BrachyLOC100828721 BrachyLOC100840882 BrachyLOC100832364 BrachyLOC100831882 BrachyLOC100841189 BrachyLOC100832679 BrachyLOC100832989 BrachyLOC100841493 BrachyLOC100833304 BrachyLOC100841799 BrachyLOC100833611 
1337:1:chr_1:- BrachyLOC100844154 BrachyLOC100825967 BrachyLOC100825654 BrachyLOC100832117 BrachyLOC100831520 BrachyLOC100829994 BrachyLOC100827255 BrachyLOC100825084 BrachyLOC100823856 BrachyLOC100821687 BrachyLOC100825042 BrachyLOC100824737 BrachyLOC100842629 BrachyLOC100840194 BrachyLOC100837549 BrachyLOC112269860 BrachyLOC100833047 BrachyLOC100824433 BrachyLOC100829993 BrachyLOC100827754 BrachyLOC100826210 BrachyLOC100825277 BrachyLOC100824126 BrachyLOC104581651 BrachyLOC100845370 BrachyLOC100822310 BrachyLOC100823513 BrachyLOC100823204 BrachyLOC100839586 

# In the drimmBlocks folder, the block information corresponding to Brachy.block (in the form of homologous gene name)
# The first column is the ID of the block
# The second column (after the first colon) is the ordinal number of the block in the species (counting from left to right, top to bottom)
# The third column (after the second colon) is the chromosome number where the block is located
# The fourth column (after the third colon) is the direction of the block, the "+" sign represents the forward direction, and the "-" sign represents the reverse direction
# The fifth column (after the first space) and later is the name of the homologous genes of the block in the complete species after LCS matching. The order of the gene names is based on the direction of the block
```


## 4.Generate the blockindex.genenumber file required for IAGS drawing

```python
sp = ['Brachy','Maize','Rice','Sorghum','Telongatum']
finalBlocks_path = './example/finalBlocks'
```
+ sp: All species name
+ finalBlocks_path: The finalBlocks folder path generated in the previous step

The output of processGenenumber is the blockindex.genenumber file, which records the correspondence between the synteny block number and the maximum number of genes in the actual species. This file can be used as the block_length_file parameter of plotChrsRearrangement in the IAGS plotting function.
+ blockindex.genenumber

```
blockID	blockLength
1590	46
1592	50
1622	16
1649	114
1635	23
1561	36
1554	175

# The first column is the block number, and the second column is the block length, where the block length is the maximum number of genes in the complete species after LCS matching.
```



## DRIMM-synteny


[DRIMM-Synteny: decomposing genomes into evolutionary conserved segments ](https://academic.oup.com/bioinformatics/article/26/20/2509/193644?login=false)

 

[DRIMM - Duplications and Rearrangements In Multiple Mammals](http://bix.ucsd.edu/projects/drimm/)
