# LAPORAN UAS SISTEM OPERASI
Nama 		: SASA SAIRA KIAY MOJO 
Kelas 		: SI A
NIM		: 05301425015

## Tugas UAS (1)
C:users\vboxuser>mkdir C:\BackupProject
C:users\vboxuser>cd C:\BackupProject
C:\BackupProject>mkdir data_asli backup recovery script
C:\BackupProject>notepad C:\BackupProject\scripts\setup.bat
 https://drive.google.com/file/d/1n_rn7vrzeBkluYDDuAkc4m4cj1prqure/view?usp=drivesdk
 
@echo off
echo === BACKUP SETUP START ===
xcopy "C:\BackupProject\data_asli" "C:\BackupProject\backup" /E /H /C /I /Y
echo Backup selesai di %date% %time%
echo === BACKUP SETUP END ===
pause

C:\BackupProject>C:\BackupProject\scripts\setup.bat

https://drive.google.com/file/d/1dioszvxbrrsr9doA7_RMDNlu6pc-moRz/view?usp=drivesdk

C:\Users\vboxuser>notepad C:\BackupProject\scripts\recovery.bat

https://drive.google.com/file/d/1O8xcH6oxoqqZefSmXBgmbQp7nfbP3MO6/view?usp=drivesdk

C:\Users\vboxuser>C:\BackupProject\scripts\recovery.bat

https://drive.google.com/file/d/11CoUabUkImg6TkCJ2NrhCdHd0TK-tbMb/view?usp=drivesdk

C:\Users\vboxuser>notepad C:\BackupProject\scripts\recovery_log.txt

https://drive.google.com/file/d/1YRwks1mY9QoaGf2FQZ_7QUZ0XTHQwzqa/view?usp=drivesdk

## Tugas UAS (2)
C:\Users\vboxuser>notepad C:\BackupProject\scripts\backupfiletertentu.bat

 @echo off
set SOURCE=C:\Data
set TARGET=D:\Backup

rem Backup hanya PDF dan DOCX
robocopy "%SOURCE%" "%TARGET%" *.pdf *.docx /S

https://drive.google.com/file/d/1h6MWgI9Ys6YOiniwa-xfbZgqvVTxnOd3/view?usp=drivesdk

C:\Users\vboxuser>C:\BackupProject\scripts\backupfiletertentu.bat
 
https://drive.google.com/file/d/1RfBcnaIE7XTj9JxsoBrKi7uzOGKsM2-o/view?usp=drivesdk

C:\Users\vboxuser>C:\BackupProject\scripts\countertambahan.bat

@echo off
setlocal enabledelayedexpansion

set SOURCE=C:\Data
set TARGET=D:\Backup

set COUNT=0

for %%F in ("%SOURCE%\*.pdf" "%SOURCE%\*.docx") do (
    set /a COUNT+=1
    echo Copying file !COUNT!: %%F
    copy "%%F" "%TARGET%"
)

https://drive.google.com/file/d/1DmVzOGUtH73n5ghJjIzHDE186uMmnHe8/view?usp=drivesdk

(gabungan seluruh script)
https://drive.google.com/file/d/1DtZ3vKVVNhe4F1DOM7xxRpbov4t6qAij/view?usp=drivesdk
https://drive.google.com/file/d/1oFUR8I0_JT0F1GvwHJyZoRzElMC3JVe9/view?usp=drivesdk

## Tugas UAS (3)
C:\Users\vboxuser>D:
D:\>echo @echo off > uas3.bat (comand untuk membuat notepad script)

 
(file notepad yang akan di isi script yang telah dibuat dari command prompt)
https://drive.google.com/file/d/14WM5O7fXMAYb8WZJvB9xfmtvLT9opMJ8/view?usp=drivesdk

@echo off
title ALL-IN-ONE BACKUP SYSTEM
setlocal enabledelayedexpansion

REM ================================
REM KONFIGURASI EMAIL (EDIT BAGIAN INI)
REM ================================
set EMAIL_TO=mipa1punya@gmail.com
set EMAIL_FROM=sasasairakiaymojoo@gmail.com@gmail.com
set EMAIL_PASS=915 347

REM ================================
REM LOKASI BACKUP
REM ================================
set SRC=%USERPROFILE%\Desktop\SimulasiDriveD
set DST=%USERPROFILE%\Desktop\BackupResults
mkdir "%DST%" 2>nul

:MENU
cls
echo ==============================================
echo           ALL-IN-ONE BACKUP SYSTEM
echo ==============================================
echo 1. Selective Backup
echo 2. Incremental Backup
echo 3. Scheduled Backup
echo 4. Exit
echo ==============================================
set /p pilih=Pilih menu (1-4): 

if "%pilih%"=="1" goto SELECTIVE
if "%pilih%"=="2" goto INCREMENTAL
if "%pilih%"=="3" goto SCHEDULE
if "%pilih%"=="4" exit

goto MENU



REM ============================================================
REM 1) SELECTIVE BACKUP
REM ============================================================
:SELECTIVE
cls
echo SELECTIVE BACKUP
echo ----------------------------------------------------
echo Folder tersedia di: %SRC%
dir "%SRC%" /b /ad
echo ----------------------------------------------------
set /p folder=Masukkan nama folder (atau * untuk semua): 

set OUT=%DST%\Selective_%date:/=-%_%time::=-%
mkdir "%OUT%"

echo Menyalin folder: %folder%
xcopy "%SRC%\%folder%" "%OUT%\%folder%" /E /H /C /I /Y >nul

echo.
echo Backup selesai. Lokasi: %OUT%

REM ============== EMAIL NOTIF OTOMATIS ==============
powershell -Command ^
"Send-MailMessage -To '%EMAIL_TO%' -From '%EMAIL_FROM%' -Subject 'Selective Backup selesai' -Body 'Selective backup folder %folder% selesai. Output: %OUT%' -SmtpServer 'smtp.gmail.com' -Port 587 -UseSsl -Credential (New-Object System.Management.Automation.PSCredential('%EMAIL_FROM%',(ConvertTo-SecureString '%EMAIL_PASS%' -AsPlainText -Force)))"

echo Email notifikasi terkirim!
pause
goto MENU



REM ============================================================
REM 2) INCREMENTAL BACKUP
REM ============================================================
:INCREMENTAL
cls
echo INCREMENTAL BACKUP
echo ----------------------------------------------------

set SNAP=%DST%\last_snapshot.txt
set NEWSNAP=%DST%\new_snapshot.txt
set OUT=%DST%\Incremental_%date:/=-%_%time::=-%
mkdir "%OUT%" 2>nul

echo Membuat snapshot baru...
(for /r "%SRC%" %%f in (*.*) do echo %%~zf %%f) > "%NEWSNAP%"

if not exist "%SNAP%" (
    echo Snapshot pertama, melakukan FULL backup...
    xcopy "%SRC%" "%OUT%" /E /H /C /I /Y >nul
    copy "%NEWSNAP%" "%SNAP%" >nul

    powershell -Command ^
    "Send-MailMessage -To '%EMAIL_TO%' -From '%EMAIL_FROM%' -Subject 'Incremental Backup (Full) selesai' -Body 'Full backup pertama selesai. Output: %OUT%' -SmtpServer 'smtp.gmail.com' -Port 587 -UseSsl -Credential (New-Object PSCredential('%EMAIL_FROM%',(ConvertTo-SecureString '%EMAIL_PASS%' -AsPlainText -Force)))"

    echo Email notifikasi terkirim!
    pause
    goto MENU
)

echo File yang berubah:
echo.
for /f "tokens=1,*" %%a in (%NEWSNAP%) do (
    find "%%b" "%SNAP%" | find "%%a" >nul
    if errorlevel 1 (
        echo Updated: %%b
        copy "%%b" "%OUT%" >nul
    )
)

copy "%NEWSNAP%" "%SNAP%" >nul

echo.
echo Incremental backup selesai. Output: %OUT%

REM ============== EMAIL NOTIF ==============
powershell -Command ^
"Send-MailMessage -To '%EMAIL_TO%' -From '%EMAIL_FROM%' -Subject 'Incremental Backup selesai' -Body 'Incremental backup selesai. Output: %OUT%' -SmtpServer 'smtp.gmail.com' -Port 587 -UseSsl -Credential (New-Object PSCredential('%EMAIL_FROM%',(ConvertTo-SecureString '%EMAIL_PASS%' -AsPlainText -Force)))"

echo Email notifikasi terkirim!
pause
goto MENU



REM ============================================================
REM 3) SCHEDULED BACKUP
REM ============================================================
:SCHEDULE
cls
echo SCHEDULED BACKUP
echo ----------------------------------------------------
set /p menit=Backup otomatis setiap berapa menit? : 

schtasks /create /tn "AutoBackupUAS" /sc minute /mo %menit% /tr "%~dp0BackupSystem_AllInOne.bat" /f

echo Jadwal backup dibuat setiap %menit% menit.

REM ============== EMAIL NOTIF ==============
powershell -Command ^
"Send-MailMessage -To '%EMAIL_TO%' -From '%EMAIL_FROM%' -Subject 'Scheduled Backup Dibuat' -Body 'Scheduled backup telah dibuat setiap %menit% menit.' -SmtpServer 'smtp.gmail.com' -Port 587 -UseSsl -Credential (New-Object PSCredential('%EMAIL_FROM%',(ConvertTo-SecureString '%EMAIL_PASS%' -AsPlainText -Force)))"

echo Email notifikasi terkirim!
pause
goto MENU
(script yang kita ketik di dalam notepad)

 https://drive.google.com/file/d/12Y1Nvq9dWMgWuJ070Kr8YrjH65GZIsgA/view?usp=drivesdk
(tampilan saat script dijalankan)

 https://drive.google.com/file/d/1QJymY63g4lmBF10xg6hxjmBR2wIOW5j9/view?usp=drivesdk
https://drive.google.com/file/d/1MojeP8aVzTMW-NFjm4kHkaFYUTLWmFGN/view?usp=drivesdk
https://drive.google.com/file/d/1Op4AAM6rrgqEAxTortRostZUVUXFehuF/view?usp=drivesdk
(folder berhasil di backup dan notif berhasil dikirim)

 https://drive.google.com/drive/folders/1eZe8FCVuGfAIoQhtM3mO7TF8wEXGFbdF
(hasil folder yang telah di backup menggunakan script)
 
https://drive.google.com/file/d/1oVJ1j_GL6RoXjxBEY-i8HgK_ZjNDNKPN/view?usp=drivesdk
https://drive.google.com/file/d/1zX-wu_jBb4CyjwGx6gRrbjf6U2FNlXdr/view?usp=drivesdk
(notif hasil backup selesai)

## Tugas UAS (4)
Laporan Tugas 4: Analisis & Dokumentasi
Nama: Sasa Saira Kiay Mojo
Nim: 05301425015
Kelas: SI A | 25
MK: Sistem Operasi
Dosen Pengampuh: Bapak. Zulhair Jidan DJ. Tamu, S.Kom., M.Kom

1. Analisis Masalah
a. Penyebab Umum Bluescreen
	Driver yang rusak atau tidak kompatibel.
	Kerusakan pada hardware seperti RAM atau HDD/SSD.
	File sistem Windows corrupt.
	Infeksi malware.
	Overheating atau power failure.
b. Kenapa CMD Masih Bisa Diakses?
	CMD berjalan di lingkungan Windows Recovery Environment (WinRE).
	WinRE tetap aktif meskipun sistem utama gagal boot.
	CMD digunakan sebagai alat perbaikan dasar seperti SFC, CHKDSK, dan BOOTREC.
2. Solusi Alternatif
a. Tool Recovery Data Lainnya
	Recuva
	EaseUS Data Recovery
	MiniTool Power Data Recovery
	TestDisk&PhotoRec
	Disk Drill
b. Cloud Backup sebagai Preventif
	Menyimpan salinan data di luar perangkat.
	Aman dari kerusakan fisik, bluescreen, atau kehilangan device.
	Memudahkan restore kapan saja.
3. Best Practice
a. Strategi Backup 3-2-1 Rule
	3 salinan data.
	2 media penyimpanan berbeda.
	1 salinan di lokasi berbeda (misalnya cloud).
b. Automasi Backup Rutin
	Menggunakan Task Scheduler.
	Menggunakan layanan cloud auto-sync.
	Menggunakan software backup otomatis seperti AOMEI atau Macrium.
4. Kesimpulan
a. Pembelajaran dari Simulasi
Dari simulasi, dapat dipahami bahwa bluescreen sering terjadi karena kerusakan sistem atau hardware. CMD pada WinRE membantu melakukan perbaikan dasar, dan proses backup sangat penting sebagai tindakan preventif.
b. Aplikasi di Dunia Nyata
Teknik recovery dan backup sangat berguna pada perusahaan, instansi pendidikan, maupun penggunaan pribadi untuk menghindari kehilangan data penting.
