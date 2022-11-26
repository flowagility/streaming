# FlowAgility Streaming Service (v.1.0.0)
Need to do the streaming of an Agility Event that is running on FlowAgility platform? No worries, we got your back! Here is all the information you need to be able to receive the best streaming data in real time. Any doubts just contact us at info@flowagility.com

## Contents
- [Streaming API](#streaming-api)
- [Flows and Statuses](#flows-and-statuses)
- [Requests & Responses](#request--response)
- [JSON messages](#json-messages)
- [Message Variables Format](#message-variables-format)
- [Example](#example)

## Streaming API
In order to receive actual data on participants, running in the certain ring, Streamer needs to connect via websocket to FA platform using the URL which is provided by the platform and which is unique to a certain event, date and ring. Information is then send as JSON message, which is having different structure, depending on the phase of the competition. The following description is covering the formats, statuses and variables, that are used in these JSON messages.

## Flows and Statuses
An Agility Event has one or more rings were all the competition takes place according to a predefined schedule. Independently of what trials and what runs happen on each ring, the information we will send is per ring per day, and fully organised according to the ring schedule. In situations where we will have “Combined Results”, they are added to the streaming information after the last run of those combined results is completed.

Playset is representing a binomial, which is running in a certain run. Playset has handler and dog information and is also showing faults, refusals, disqualification and time, as well as classification, speed and total penalizations. Playset may have several states. Each run starts with the first playset `ready` to run. At events where timer is connected to the platform, as soon as the dog crosses the start gates, the playset is set to `running`. On events without the timer connected, playset may be set to running with the first fault, refusal or when binom is eliminated. Playset is set into `calculated`, when binomial data is submitted by the Judge Helper to the platform.

JSON messages are sent on any updated of active run and also when binomial is getting a fault, refusal, disqualification or when timer starts and stops (for events with timer only). JSON message is usually covering the run, where the binomial is ready/running/calculated and the next binomial - whether it is in the current run or in a new one - this information is presented in a `run_ready` structure. If `run` and `run_ready` runs names are equal, then the next binomial is still from the same run, otherwise the next binomial is from another run.

After the playset is submitted, platform is doing calculation of the results and sends up to 10 best results for this run together with calculated current playset data.

## Request / Response
There are 3 requests that are supported by the FA platform at the moment:
- `ping` - message to check the connection with the server and to keep connection open. Should be sent with the interval of less than 60 seconds. Platform will response with the message `pong`
- `streaming_data` - message to be sent at any moment to receive the JSON message with the current data on the run and playset.
- `version` - response to this message is a current version of the streaming API.

All other JSON messages from the platform are generated and broadcasted when run/playset information was changed on the Platform.


## JSON Messages
The following examples are representing JSON messages which will be broadcasted during the event.

### 1. When CI (course info) and SO (start order) is not set for the first run of the day/ring
```json
{
  "error": "Runs are not ready yet"
}
```

### 2. When SO of the first run for the date/ring is set
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": "",
  "run_ready": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Pawer",
      "country": "ES",
      "disqualification": "elim",
      "dog_age": "6 years",
      "dog_breed": "Shetland Sheepdog",
      "dog_family_name": "Flow",
      "dog_gender": "Male",
      "dog_height": "34.0",
      "dog_studbook_name": "Lasborras Fifty Shades",
      "dorsal": "9",
      "faults": "0",
      "handler": "Julen Lázaro Martínez",
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",
      "qualification": "",
      "refusals": "0",
      "speed": "",
      "start_order": "1",
      "status_string": "ready",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "timestamp": "2022-09-04 22:55:39"
}
```

### 3. When first playset is getting the first Fault
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Baix Llobregat",
      "country": "ES",
      "disqualification": "elim",
      "dog_age": "11 years",
      "dog_breed": "Poodle",
      "dog_family_name": "Mel",
      "dog_gender": "Female",
      "dog_height": "32.0",
      "dog_studbook_name": "Mel De Torkental",
      "dorsal": "41",
      "faults": "1",
      "handler": "Anna Nogues Lopez",
      "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
      "qualification": "",
      "refusals": "0",
      "speed": "",
      "start_order": "1",
      "status_string": "running",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "results_best": [],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "run_ready": "",
  "timestamp": "2022-09-05 00:32:38"
}
```

When first playset is getting the Refusal
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Baix Llobregat",
      "country": "ES",
      "disqualification": "elim",
      "dog_age": "11 years",
      "dog_breed": "Poodle",
      "dog_family_name": "Mel",
      "dog_gender": "Female",
      "dog_height": "32.0",
      "dog_studbook_name": "Mel De Torkental",
      "dorsal": "41",
      "faults": "1",
      "handler": "Anna Nogues Lopez",
      "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
      "qualification": "",
      "refusals": "1",
      "speed": "",
      "start_order": "1",
      "status_string": "running",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "results_best": [],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "run_ready": "",
  "timestamp": "2022-09-05 00:33:23"
}
```

### 4. When first playset is disqualified by the Judge
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Baix Llobregat",
      "country": "ES",
      "disqualification": "elim",
      "dog_age": "11 years",
      "dog_breed": "Poodle",
      "dog_family_name": "Mel",
      "dog_gender": "Female",
      "dog_height": "32.0",
      "dog_studbook_name": "Mel De Torkental",
      "dorsal": "41",
      "faults": "0",
      "handler": "Anna Nogues Lopez",
      "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
      "qualification": "",
      "refusals": "0",
      "speed": "",
      "start_order": "1",
      "status_string": "running",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "results_best": [],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "run_ready": "",
  "timestamp": "2022-09-05 00:34:25"
}
```

### 5. When first playset is submitted with 2F and 1R and time of 23.56
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "1",
      "club": "Baix Llobregat",
      "country": "ES",
      "disqualification": "",
      "dog_age": "11 years",
      "dog_breed": "Poodle",
      "dog_family_name": "Mel",
      "dog_gender": "Female",
      "dog_height": "32.0",
      "dog_studbook_name": "Mel De Torkental",
      "dorsal": "41",
      "faults": "2",
      "handler": "Anna Nogues Lopez",
      "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
      "qualification": "V_GOOD",
      "refusals": "1",
      "speed": "8.49 m/s",
      "start_order": "1",
      "status_string": "calculated",
      "time": "23.56",
      "time_marker": "true",
      "total_penalization": "15.00"
    },
    "results_best": [
      {
        "classification": "1",
        "club": "Baix Llobregat",
        "country": "ES",
        "disqualification": "",
        "dog_age": "11 years",
        "dog_breed": "Poodle",
        "dog_family_name": "Mel",
        "dog_gender": "Female",
        "dog_height": "32.0",
        "dog_studbook_name": "Mel De Torkental",
        "dorsal": "41",
        "faults": "2",
        "handler": "Anna Nogues Lopez",
        "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
        "qualification": "V_GOOD",
        "refusals": "1",
        "speed": "8.49 m/s",
        "start_order": "1",
        "status_string": "calculated",
        "time": "23.56",
        "time_marker": "true",
        "total_penalization": "15.00"
      }
    ],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "run_ready": {
    "name": "G3 / S",
    "playset": {
      "classification": "1",
      "club": "Pawer",
      "country": "ES",
      "disqualification": "",
      "dog_age": "6 years",
      "dog_breed": "Shetland Sheepdog",
      "dog_family_name": "Flow",
      "dog_gender": "Male",
      "dog_height": "34.0",
      "dog_studbook_name": "Lasborras Fifty Shades",
      "dorsal": "9",
      "faults": "0",
      "handler": "Julen Lázaro Martínez",
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",
      "qualification": "EXC_0",
      "refusals": "0",
      "speed": "",
      "start_order": "2",
      "status_string": "ready",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "timestamp": "2022-09-05 00:36:12"
}
```

### 6. When second playset of the 2-playsets run (1st run-pair - without combined results) is submitted
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "1",
      "club": "Pawer",
      "country": "ES",
      "disqualification": "",
      "dog_age": "6 years",
      "dog_breed": "Shetland Sheepdog",
      "dog_family_name": "Flow",
      "dog_gender": "Male",
      "dog_height": "34.0",
      "dog_studbook_name": "Lasborras Fifty Shades",
      "dorsal": "9",
      "faults": "1",
      "handler": "Julen Lázaro Martínez",
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",
      "qualification": "EXC",
      "refusals": "0",
      "speed": "6.95 m/s",
      "start_order": "2",
      "status_string": "calculated",
      "time": "28.78",
      "time_marker": "true",
      "total_penalization": "5.00"
    },
    "results_best": [
      {
        "classification": "1",
        "club": "Pawer",
        "country": "ES",
        "disqualification": "",
        "dog_age": "6 years",
        "dog_breed": "Shetland Sheepdog",
        "dog_family_name": "Flow",
        "dog_gender": "Male",
        "dog_height": "34.0",
        "dog_studbook_name": "Lasborras Fifty Shades",
        "dorsal": "9",
        "faults": "1",
        "handler": "Julen Lázaro Martínez",
        "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",
        "qualification": "EXC",
        "refusals": "0",
        "speed": "6.95 m/s",
        "start_order": "2",
        "status_string": "calculated",
        "time": "28.78",
        "time_marker": "true",
        "total_penalization": "5.00"
      },
      {
        "classification": "2",
        "club": "Baix Llobregat",
        "country": "ES",
        "disqualification": "",
        "dog_age": "11 years",
        "dog_breed": "Poodle",
        "dog_family_name": "Mel",
        "dog_gender": "Female",
        "dog_height": "32.0",
        "dog_studbook_name": "Mel De Torkental",
        "dorsal": "41",
        "faults": "2",
        "handler": "Anna Nogues Lopez",
        "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
        "qualification": "V_GOOD",
        "refusals": "1",
        "speed": "8.49 m/s",
        "start_order": "1",
        "status_string": "calculated",
        "time": "23.56",
        "time_marker": "false",
        "total_penalization": "15.00"
      }
    ],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "run_ready": "",
  "timestamp": "2022-09-05 00:37:30"
}
```

### 7. When the 2nd run-pair SO is set (run-ready information is showing the run which is ready to start)
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "1",
      "club": "Pawer",
      "country": "ES",
      "disqualification": "",
      "dog_age": "6 years",
      "dog_breed": "Shetland Sheepdog",
      "dog_family_name": "Flow",
      "dog_gender": "Male",
      "dog_height": "34.0",
      "dog_studbook_name": "Lasborras Fifty Shades",
      "dorsal": "9",
      "faults": "1",
      "handler": "Julen Lázaro Martínez",
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",
      "qualification": "EXC",
      "refusals": "0",
      "speed": "6.95 m/s",
      "start_order": "2",
      "status_string": "calculated",
      "time": "28.78",
      "time_marker": "true",
      "total_penalization": "5.00"
    },
    "results_best": [
      {
        "classification": "1",
        "club": "Pawer",
        "country": "ES",
        "disqualification": "",
        "dog_age": "6 years",
        "dog_breed": "Shetland Sheepdog",
        "dog_family_name": "Flow",
        "dog_gender": "Male",
        "dog_height": "34.0",
        "dog_studbook_name": "Lasborras Fifty Shades",
        "dorsal": "9",
        "faults": "1",
        "handler": "Julen Lázaro Martínez",
        "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",
        "qualification": "EXC",
        "refusals": "0",
        "speed": "6.95 m/s",
        "start_order": "2",
        "status_string": "calculated",
        "time": "28.78",
        "time_marker": "true",
        "total_penalization": "5.00"
      },
      {
        "classification": "2",
        "club": "Baix Llobregat",
        "country": "ES",
        "disqualification": "",
        "dog_age": "11 years",
        "dog_breed": "Poodle",
        "dog_family_name": "Mel",
        "dog_gender": "Female",
        "dog_height": "32.0",
        "dog_studbook_name": "Mel De Torkental",
        "dorsal": "41",
        "faults": "2",
        "handler": "Anna Nogues Lopez",
        "id": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
        "qualification": "V_GOOD",
        "refusals": "1",
        "speed": "8.49 m/s",
        "start_order": "1",
        "status_string": "calculated",
        "time": "23.56",
        "time_marker": "false",
        "total_penalization": "15.00"
      }
    ],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "JP"
  },
  "run_ready": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Baix Llobregat",
      "country": "ES",
      "disqualification": "",
      "dog_age": "11 years",
      "dog_breed": "Poodle",
      "dog_family_name": "Mel",
      "dog_gender": "Female",
      "dog_height": "32.0",
      "dog_studbook_name": "Mel De Torkental",
      "dorsal": "41",
      "faults": "0",
      "handler": "Anna Nogues Lopez",
      "id": "wwwwwwww-wwww-wwww-wwww-wwwwwwwwwwww",
      "qualification": "",
      "refusals": "0",
      "speed": "",
      "start_order": "1",
      "status_string": "ready",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "trial_name": "RSCE 4 Septiembre ",
    "type": "AG"
  },
  "timestamp": "2022-09-05 00:39:52"
}
```

### 8. When 2nd run-pair 1st playset got first Fault
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Baix Llobregat",
      "country": "ES",
      "disqualification": "",
      "dog_age": "11 years",
      "dog_breed": "Poodle",
      "dog_family_name": "Mel",
      "dog_gender": "Female",
      "dog_height": "32.0",
      "dog_studbook_name": "Mel De Torkental",
      "dorsal": "41",
      "faults": "1",
      "handler": "Anna Nogues Lopez",
      "id": "wwwwwwww-wwww-wwww-wwww-wwwwwwwwwwww",
      "qualification": "",
      "refusals": "0",
      "speed": "",
      "start_order": "1",
      "status_string": "running",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "results_best": [],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "AG"
  },
  "run_ready": "",
  "timestamp": "2022-09-05 00:41:22"
}
```

### 9. When 2nd run-pair 1st playset is submitted
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "1",
      "club": "Baix Llobregat",
      "country": "ES",
      "disqualification": "",
      "dog_age": "11 years",
      "dog_breed": "Poodle",
      "dog_family_name": "Mel",
      "dog_gender": "Female",
      "dog_height": "32.0",
      "dog_studbook_name": "Mel De Torkental",
      "dorsal": "41",
      "faults": "1",
      "handler": "Anna Nogues Lopez",
      "id": "wwwwwwww-wwww-wwww-wwww-wwwwwwwwwwww",
      "qualification": "EXC",
      "refusals": "0",
      "speed": "6.90 m/s",
      "start_order": "1",
      "status_string": "calculated",
      "time": "29.00",
      "time_marker": "true",
      "total_penalization": "5.00"
    },
    "results_best": [
      {
        "classification": "1",
        "club": "Baix Llobregat",
        "country": "ES",
        "disqualification": "",
        "dog_age": "11 years",
        "dog_breed": "Poodle",
        "dog_family_name": "Mel",
        "dog_gender": "Female",
        "dog_height": "32.0",
        "dog_studbook_name": "Mel De Torkental",
        "dorsal": "41",
        "faults": "1",
        "handler": "Anna Nogues Lopez",
        "id": "wwwwwwww-wwww-wwww-wwww-wwwwwwwwwwww",
        "qualification": "EXC",
        "refusals": "0",
        "speed": "6.90 m/s",
        "start_order": "1",
        "status_string": "calculated",
        "time": "29.00",
        "time_marker": "true",
        "total_penalization": "5.00"
      }
    ],
    "results_combined": [],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "AG"
  },
  "run_ready": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Pawer",
      "country": "ES",
      "disqualification": "",
      "dog_age": "6 years",
      "dog_breed": "Shetland Sheepdog",
      "dog_family_name": "Flow",
      "dog_gender": "Male",
      "dog_height": "34.0",
      "dog_studbook_name": "Lasborras Fifty Shades",
      "dorsal": "9",
      "faults": "0",
      "handler": "Julen Lázaro Martínez",
      "id": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
      "qualification": "",
      "refusals": "0",
      "speed": "",
      "start_order": "2",
      "status_string": "ready",
      "time": "",
      "time_marker": "false",
      "total_penalization": ""
    },
    "trial_name": "RSCE 4 Septiembre ",
    "type": "AG"
  },
  "timestamp": "2022-09-05 00:42:45"
}
```

### 10. When 2nd run-pair last playset is submitted
```json
{
  "event_name": "RSCE Pawer 26-27-28 Agosto / 2-3-4 Septiembre",
  "ring_name": "1",
  "run": {
    "name": "G3 / S",
    "playset": {
      "classification": "",
      "club": "Pawer",
      "country": "ES",
      "disqualification": "elim",
      "dog_age": "6 years",
      "dog_breed": "Shetland Sheepdog",
      "dog_family_name": "Flow",
      "dog_gender": "Male",
      "dog_height": "34.0",
      "dog_studbook_name": "Lasborras Fifty Shades",
      "dorsal": "9",
      "faults": "0",
      "handler": "Julen Lázaro Martínez",
      "id": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
      "qualification": "DISQualification",
      "refusals": "0",
      "speed": "",
      "start_order": "2",
      "status_string": "calculated",
      "time": "",
      "time_marker": "false",
      "total_penalization": "100.00"
    },
    "results_best": [
      {
        "classification": "1",
        "club": "Baix Llobregat",
        "country": "ES",
        "disqualification": "",
        "dog_age": "11 years",
        "dog_breed": "Poodle",
        "dog_family_name": "Mel",
        "dog_gender": "Female",
        "dog_height": "32.0",
        "dog_studbook_name": "Mel De Torkental",
        "dorsal": "41",
        "faults": "1",
        "handler": "Anna Nogues Lopez",
        "id": "wwwwwwww-wwww-wwww-wwww-wwwwwwwwwwww",
        "qualification": "EXC",
        "refusals": "0",
        "speed": "6.90 m/s",
        "start_order": "1",
        "status_string": "calculated",
        "time": "29.00",
        "time_marker": "true",
        "total_penalization": "5.00"
      },
      {
        "classification": "",
        "club": "Pawer",
        "country": "ES",
        "disqualification": "elim",
        "dog_age": "6 years",
        "dog_breed": "Shetland Sheepdog",
        "dog_family_name": "Flow",
        "dog_gender": "Male",
        "dog_height": "34.0",
        "dog_studbook_name": "Lasborras Fifty Shades",
        "dorsal": "9",
        "faults": "0",
        "handler": "Julen Lázaro Martínez",
        "id": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
        "qualification": "DISQualification",
        "refusals": "0",
        "speed": "",
        "start_order": "2",
        "status_string": "calculated",
        "time": "",
        "time_marker": "false",
        "total_penalization": "100.00"
      }
    ],
    "results_combined": [
      {
        "classification": "1",
        "club": "Baix Llobregat",
        "country": "ES",
        "dog_age": "11 years",
        "dog_breed": "Poodle",
        "dog_family_name": "Mel",
        "dog_gender": "Female",
        "dog_height": "32.0",
        "dog_studbook_name": "Mel De Torkental",
        "dorsal": "41",
        "handler": "Anna Nogues Lopez",
        "id": "nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn",
        "time": "52.56",
        "total_penalization": "20.00"
      },
      {
        "classification": "2",
        "club": "Pawer",
        "country": "ES",
        "dog_age": "6 years",
        "dog_breed": "Shetland Sheepdog",
        "dog_family_name": "Flow",
        "dog_gender": "Male",
        "dog_height": "34.0",
        "dog_studbook_name": "Lasborras Fifty Shades",
        "dorsal": "9",
        "handler": "Julen Lázaro Martínez",
        "id": "mmmmmmmm-mmmm-mmmm-mmmm-mmmmmmmmmmmm",
        "time": "28.78",
        "total_penalization": "105.00"
      }
    ],
    "trial_name": "RSCE 4 Septiembre ",
    "type": "AG"
  },
  "run_ready": "",
  "timestamp": "2022-09-05 00:43:47"
}
```


## Message Variables Format
The following section describes JSON messages format and variables, that are broadcasted during event.

Message when the start order (SO) for the first run for that date and ring is not set yet:
- `error` - has only one value `Runs are not ready yet`

Event structure:
- `event_name` - name of the event
- `ring_name` - ring name, represented by `Integer` starting from 1
- `run` - structure, to keep information about the run, where the playset is currently running
- `run_ready` - structure to keep information about the run, which is ready to be started
- `timestamp` - UTC date and time when the message was sent

Run structure:
- `name` - run's name without type, for example `G3 / S`
- `playset` - structure that represents playset information
- `results_best` - array of the playsets, representing 10 best results - it is shown after the 1st submitted playset in this run
- `results_combined` - array of the combined results - shown after the last playset of the last brother-run is submitted
- `trial_name` - name of the trial, to which current run belongs
- `type` - type of the run, can be `AG` for agility and `JP` for jumping   

Playset structure:
- `classification` - classificationification, can be either empty string `""` or `Integer` starting from 1
- `club` - club of the participant
- `country` - 2-letter abbreviation of the country of the participant
- `disqualification` - can be either empty string `""` or `elim` in case if binom was disqualificationualificationified
- `dog_age` - age of the dog in `months` or `years`
- `dog_breed` - dog's breed
- `dog_family_name` - short dog's name
- `dog_gender` - dog's gender, can be ["Female", "Male"]
- `dog_height` - god's height in cm
- `dog_studbook_name` - name of the dog according to Studbook
- `dorsal` - the unique to this event registration number of the binomial, represented as `Integer`, starts from 1
- `faults` - number of faults, given to binomial
- `handler` - full name of the handler of the dog
- `id` - unique identificator of the playset
- `qualification` - shows qualification of the playset, can be in the following range ["EXC_0", "EXC", "V_GOOD", "GOOD", "NQ", "DISQ (R)", "DISQ (T)", "DISQ"], where:
  - `EXC_0` - excellent zero
  - `EXC` - excellent
  - `V_GOOD` - very good
  - `GOOD` - good
  - `NQ` - non-qualified
  - `DISQ (R)` - disqualified by number of refusals
  - `DISQ (T)` - disqualified by exceeding MCT
  - `DISQ` - disqualified by the Judge
- `refusals` - number of refulasl given to binomial
- `speed` - speed of the dog, represented in m/s
- `start_order` - start order of the binomial in this run 
- `status_string` - status of the current playset, can be in the following range ["pending", "ready", "running", "calculated"], where:
  - `pending` - is not next to run
  - `ready` - is next to run
  - `running` - currently running
  - `calculated` - already finished with calculated results
- `time` - time of the dog, represented with 2 or 3 decimals depending on the Federation requirements
- `time_marker` - shows that dog marked the time, can be ["true", "false"]
- `total_penalization` - total penalization time in seconds with 2 or 3 decimals depending on the Federation requirements


## Example
FlowAgility streaming Demo, created by [@malinantonsson](https://github.com/malinantonsson) can be found [here](https://github.com/malinantonsson/flow-agility-streaming-demo)


