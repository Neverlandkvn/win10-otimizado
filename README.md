# win10-otimizado
abra o bloco de nota e salve como (OTIMIZAR_WINDOWS.bat) tipo todos os arquivos 


@echo off
title OTIMIZACAO WINDOWS 10
color 0A

:: Verificar administrador
net session >nul 2>&1
if %errorlevel% neq 0 (
    echo Execute este arquivo como ADMINISTRADOR!
    pause
    exit
)

echo =====================================
echo       OTIMIZADOR WINDOWS 10
echo =====================================
echo.

:: Criar ponto de restauracao
echo Criando ponto de restauracao...
powershell -NoProfile -Command "Checkpoint-Computer -Description 'Antes da Otimizacao' -RestorePointType MODIFY_SETTINGS" 2>nul

:: Limpar arquivos temporarios
echo Limpando arquivos temporarios...
del /f /s /q "%TEMP%\*" 2>nul
del /f /s /q "C:\Windows\Temp\*" 2>nul

:: Limpar cache DNS
echo Limpando cache DNS...
ipconfig /flushdns

:: Desativar aplicativos em segundo plano
echo Desativando aplicativos em segundo plano...
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications" /v GlobalUserDisabled /t REG_DWORD /d 1 /f

:: Desativar sugestoes e publicidade
echo Desativando sugestoes...
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f

:: Melhorar desempenho visual
echo Ajustando efeitos visuais...
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects" /v VisualFXSetting /t REG_DWORD /d 2 /f

:: Desativar telemetria opcional
echo Ajustando telemetria...
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection" /v AllowTelemetry /t REG_DWORD /d 1 /f

:: Ativar plano de energia alto desempenho
echo Configurando energia...
powercfg /setactive SCHEME_MIN

:: Otimizar unidade
echo Otimizando unidade C...
defrag C: /O

:: Limpeza de componentes antigos
echo Limpando componentes do Windows...
DISM /Online /Cleanup-Image /StartComponentCleanup

:: Limpeza de disco
cleanmgr /verylowdisk

echo.
echo =====================================
echo OTIMIZACAO CONCLUIDA!
echo Reinicie o computador.
echo =====================================
pause
