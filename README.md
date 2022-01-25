README for package: custom_hyl

custom_hyl is a top level controller:

1. Capture runs Continuously in Burst Mode
2. SR=2MHz, SSB=4x2+4x4 = 20bytes = 40MB/s peak
3. Bursts are triggered at 48Hz, TLEN=8192, avg rate: 8/40 * 40 = 8MB/s

Host data application acq400_stream streams data from port 4210

4. Port 4210 is nailed up for long duration.

Data flow within the shot is controlled by 

4.1 Embedded script HYCON
4.2 Embedded service HY-comm at port 61001
=> NEW filename

4.3 Embedded service HY-stat at port 61002
NEW filename =>

5. Sequence

5.1 Customer controller sends "NEW filename" command to ACQ

5.2 HYCON responds to NEW:
- disables FACET gate source
- enable TRG signal
- posts NEW filename message to HY-status
- responds to TRG with low latency, enabling FACET
- data flows immediately on the next FACET

5.3 acq400_stream
- opens a socket to 4210 and streams data as normal
- opens a socket to HY-status
- responds to a NEW message on HY-status by starting a new file


6. Host Side Scripts

6.1 HOST/hytest : automates a session
6.2 HOST/extract_csv : extracts timing data from a session log


