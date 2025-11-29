My komorebi command cheat sheet:


# Start komorebi, komorebi bar & whkd : 
```
  komorebic start --whkd --bar
```

# Stop komorebi, komorebi bar & whkd : 
```
komorebic stop
```
or
```
  Stop-Process -Name komorebi-bar -Force -ErrorAction SilentlyContinue  
  Stop-Process -Name whkd -Force -ErrorAction SilentlyContinue  
  Stop-Process -Name komorebi -Force -ErrorAction SilentlyContinue
```

# Stop/start komorebi bar only :
```
  Stop-Process -Name "komorebi-bar" -Force  
  Start-Process komorebi-bar
```

# Cheat sheet for komorebi
```
Alt + i
```

# Starts Komorebi immediately when the W11 session loads
1. Open Task Scheduler

```
Win + R → taskschd.msc
```
2. Create a new task

```
Action → Create Task… (NOT "Create Basic Task")
```
3. General tab

```
Name: Komorebi
Run only when user is logged on
Run with highest privileges
```

4. Triggers tab

```
New → Begin the task: At log on
```
5. Actions tab (adds the following to start komorebi, komorebi bar, whkd & wait 2 seconds before setting border-style to square)

```
Action: Start a program
Program/script: powershell.exe
Arguments: -WindowStyle Hidden -ExecutionPolicy Bypass -Command "Start-Process 'C:\Program Files\komorebi\bin\komorebic.exe' -ArgumentList 'start --whkd --bar'; Start-Sleep -Seconds 2; Start-Process 'C:\Program Files\komorebi\bin\komorebic.exe' -ArgumentList 'border-style square' >$null 2>&1"
```

6. Conditions / Settings

```
Uncheck "Start only if on AC power"
Enable "Start the task as soon as possible after a scheduled start is missed"
```






# Force restart of Komorebi Bar when W11 goes out of sleep
1. Create a file named restart-komorebi.cmd (i created it in the komorebi program folder) containing:

```
taskkill /IM komorebi-bar.exe /F
start "" "C:\Program Files\komorebi\bin\komorebi-bar.exe"
timeout /t 1 >nul
```

2. Create a Task Scheduler trigger:

```
Win + R → taskschd.msc
Task Scheduler → Create Task
Triggers → New…
Begin the task: On an event
Log: System
Source: Power-Troubleshooter
Event ID: 1 (resume from sleep)
```

3. Actions tab
```
Action: Start a program
Program/script: "C:\Program Files\komorebi\restart-komorebi.cmd"
```
