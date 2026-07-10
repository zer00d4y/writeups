# Fools Mate Write-up

<img src="https://tryhackme-images.s3.amazonaws.com/room-icons/62ff64c3c859dc0042b2b9f6-1782993874301" width="200" height="200">

--------------------------------------------------------------------------------------------------------------------------------

<img width="1000" height="600" alt="image" src="https://github.com/user-attachments/assets/8ea1418d-224c-4b13-b858-531f056a629f" />

<img width="1000" height="600" alt="image" src="https://github.com/user-attachments/assets/d543d3a3-7481-42f3-a2a3-2057e6d10e5a" />

view-source:http://10.112.138.159/js/app.js

<img width="600" height="220" alt="image" src="https://github.com/user-attachments/assets/42815c3a-5e2b-40ca-8102-bcaf5e90a978" />

<img width="497" height="544" alt="image" src="https://github.com/user-attachments/assets/5cfddfbd-8d2b-4200-9a39-87d3da3ca801" />

<img width="992" height="551" alt="image" src="https://github.com/user-attachments/assets/f358aa32-b2a4-4c57-a7bd-cb4f58b6944c" />

Or one-line solution with `curl`:

    curl -s -c cookies.txt -b cookies.txt -X POST http://$target/api/move -H "Content-Type: application/json" -d '{"from":"a1","to":"a8"}'

<img width="1894" height="52" alt="image" src="https://github.com/user-attachments/assets/30d4f1fd-e98d-438d-ae40-ef7b02015056" />

FLAG:

    THM{cl13nt_s1d3_ch3ckm4t3}
