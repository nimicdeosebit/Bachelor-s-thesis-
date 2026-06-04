# EEG Experiment Workflow (Muse + MuseLSL + PsychoPy + LabRecorder)

## Overview

This experiment uses:

- Muse EEG headset
- MuseLSL for streaming EEG data
- PsychoPy for stimulus presentation and behavioral responses
- LabRecorder for synchronized recording of EEG and event markers

The workflow is:

Muse EEG → MuseLSL → LabRecorder  
PsychoPy Markers → LabRecorder

LabRecorder records both EEG signals and experimental markers into a single `.xdf` file for later analysis.

---

## 1. Connect the Muse Headset

1. Put the Muse headset on your head.
   - Forehead sensors must touch the skin.
   - Ear sensors should sit firmly behind the ears.

2. Turn on the Muse headset.
   - Press and hold the power button.
   - Wait until the LED indicates the headset is powered on and ready to pair.

3. Ensure Bluetooth is enabled on the computer.

---

## 2. Start EEG Streaming

Open **Terminal 1**:

```bash
conda activate psychopy
python -m muselsl stream
```

Expected output:

```text
Looking for Muse...
Found Muse XXXXX
Connected.
Streaming...
```

Leave this terminal running for the entire experiment session.

---

## 3. Verify EEG Signal (Optional)

Open **Terminal 2**:

```bash
conda activate psychopy
python -m muselsl view
```

You should see live EEG traces.

Once you confirm that the signal looks correct, close the viewer window.

---

## 4. Start LabRecorder

Open **LabRecorder**.

You should see an EEG stream such as:

```text
Muse
```

or

```text
EEG
```

Do not start recording yet.

---

## 5. Run the Experiment

Open **Terminal 3**.

### Experiment 1

```bash
conda activate psychopy
python /Users/iuliabugan/Desktop/thesis/experiments/experiment1.py
```

### Experiment 2

```bash
conda activate psychopy
python /Users/iuliabugan/Desktop/thesis/experiments/experiment2.py
```

When the PsychoPy script starts, it creates a second LSL stream:

```text
Markers
```

---

## 6. Verify Streams in LabRecorder

LabRecorder should now display:

```text
Muse EEG
Markers
```

Select both streams.

Choose an output filename, for example:

```text
participant_001.xdf
```

Click **Start Recording**.

---

## 7. Run the Participant

Participant workflow:

1. Enter participant information.
2. Press **SPACE** to begin.
3. Watch the stimulus video(s).
4. Make a voting/response choice.
5. View the thank-you screen.

During the experiment, markers are automatically sent to LabRecorder.

Example markers:

```text
start
s1_start
s1_end
s2_start
s2_end
...
s18_start
s18_end
vote_c
end
```

These markers are synchronized with the EEG recording.

---

## 8. Finish the Participant

After the thank-you screen:

1. Stop recording in LabRecorder.
2. Save the `.xdf` file.

Behavioral responses are automatically appended to:

```text
voting_choices_experiment1.csv
```

(or the corresponding file for Experiment 2).

---

## 9. Start the Next Participant

For a new participant:

1. Leave the MuseLSL stream running.
2. Start a new recording in LabRecorder.
3. Run the PsychoPy script again.

You do **not** need to reconnect the Muse headset unless the Bluetooth connection drops.

---

# Quick Start Checklist

```text
1. Start MuseLSL stream
      ↓
2. Open LabRecorder
      ↓
3. Run PsychoPy experiment
      ↓
4. Verify EEG + Markers streams
      ↓
5. Start recording
      ↓
6. Run participant
      ↓
7. Stop recording
      ↓
8. Save .xdf file
      ↓
9. Repeat for next participant
```

---

## Troubleshooting

### Muse not found

Make sure:

- Muse is powered on.
- Bluetooth is enabled.
- No other application is connected to the headset.

Restart:

```bash
python -m muselsl stream
```

### No EEG stream in LabRecorder

Verify that MuseLSL is running:

```bash
python -m muselsl stream
```

You should see:

```text
Connected.
Streaming...
```

### No Marker stream in LabRecorder

Verify that the PsychoPy experiment is running successfully and that no errors occurred during startup.

### Connection Drops

1. Stop the experiment.
2. Restart MuseLSL:

```bash
python -m muselsl stream
```

3. Confirm the EEG signal with:

```bash
python -m muselsl view
```

4. Restart LabRecorder recording.
5. Run the experiment again.

---

## 10. Read and Inspect Recorded XDF Files

After recording a participant, you can inspect the recorded EEG and marker streams using `xdf_reader.py`.


```bash
conda activate psychopy
python ~/Desktop/thesis/processing/xdf_reader.py
```

## 11. Aggregate voting choices

When the participants have been recorded, combine all the voting choices for statistics.

```bash
conda activate psychopy
python ~/Desktop/thesis/processing/aggregate_voting_choices.py
```


