# Nmap results (transcribed from screenshots)
Raw text files were not saved. Re-run with `-oN file.txt` and drop them in this folder if you want the originals.

Host discovery `nmap -sn 10.0.0.0/24`: 10.0.0.1, 10.0.0.2, 10.0.0.4 up (3 hosts, 4.74 s)
OS `-O`: Linux 2.6.9 - 2.6.33, 1 hop
Open TCP (23): 21,22,23,25,53,80,111,139,445,512,513,514,1099,1524,2049,2121,3306,5432,5900,6000,6667,8009,8180
Open UDP: 53, 137. open|filtered: 68,69,138,514,1434,1900,4500,49152
After hardening (11:07): 21,23,139,445,1099,1524,2121,6667 all closed
