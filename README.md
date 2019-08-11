# popen_timetrigger
run_cmd_live_update-save_with_time_trigger_popen

import subprocess
import sys
import os
import threading

cwd = os.getcwd()
os.chdir('{}'.format(cwd))

def rns_time_trigger():
    global lag_trigger
    lag_trigger = True
    proc.terminate()
    
    
    
    output = []
# time trigger it triggers after time = 6*points(calculated points)
clock = threading.Timer(time, rns_time_trigger)
lag_trigger = False
clock.start()
with subprocess.Popen(w_cmd, stdout=subprocess.PIPE, bufsize=1) as proc:
    for line in proc.stdout:
        sys.stdout.buffer.write(line)
        sys.stdout.buffer.flush()
        dec = line.decode('utf-8')
        output.append(dec)
        if lag_trigger is True:
            if len(output[-2]) is not len(output[-1]):
                del output[-1]
            proc.terminate()
            break
proc.wait()
clock.cancel()
