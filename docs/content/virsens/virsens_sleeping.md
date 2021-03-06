## Sleeping Virtual Sensor Implementation Statement

**Sleeping virtual sensor** refers `activity recongination virtual sensor` status, system `power state` and system `real time info`;
they are able to assist sleeping virtual sensor calculates data precisely. Before activating **sleeping virtual sensor**, Host processor
first subscribes `activity recongination virtual sensor`, then deliver system `power state` and `real time info`.

For instance, the illustrated figure expresses sleeping virtual sensor ative working scenario.

<br>
<img src="virsens_sleeping_implementation.png"
width="70%"
height="70%"
alt="virsens sleeping implementation"
align=center />
<br>

+ Host processor delivers **system power state** to sensor hub API

```text
typedef enum {
  SYS_PWR_AWAKE = 1,    /*Device screen is being on; Host processor is working*/
  SYS_PWR_SUSPEND.      /*Device screen is being off; Host process is suspending*/
  SYS_PWR_CHARGING,     /*Device is charging via USB cable*/
  SYS_PWR_DISCHARGING,  /*Device is discharging*/
  SYS_PWR_OPERATION,    /*User is using device*/
  SYS_PWR_LEAVE_SLEEP,  /*Host processor requires sensor hub terminates sleeping processing*/
} pwr_state_id_t;

struct sensor_hub_sensor_params_t params;
pwr_state_id_t pwr_state = SYS_PWR_SUSPEND;

params.cmd = SH_CMD_HOST_SET_PARAM_POWER_STATUS;
params.data_addr = &pwr_state;
sensor_manager_setSensorParameters(SENSOR_TYPE_SLEEPING,
    &params, (sizeof(enum sensorhub_cmd_t)+sizeof(uint32_t)));
```

+ Host processor delivers **real time info** to sensor hub API

```text
 struct system_time_t time;
 time.day = 1109;   /*NOV/09*/
 time.time = 1630;  /*PM 4:30*/
 params.cmd = SH_CMD_HOST_SET_PARAM_DATE_TIME;
 params.data_addr = &time;
 sensor_manager_setSensorParameters(SENSOR_TYPE_SLEEPING,
     &params, (sizeof(enum sensorhub_cmd_t)+sizeof(struct system_time_t)));
```
