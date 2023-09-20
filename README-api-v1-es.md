# SingularHub WebSocket API v1

El servicio estará accesible desde la dirección IP (estática o dinámica) del _SingularHub_, o a través de su nombre de red local, gracias al DNS dinámico (mDNS) y conociendo el número de serie (ID de fabricación) del _SingularHub_:

`ws://SINGULAR_HUB_IP:80/api/v1`

`ws://singular-hub-ID.local:80/api/v1`

## Peticiones (Command)

Las peticiones se realizan enviando por el websocket un objeto JSON que tendrá dos atributos:

1. ```command```: siempre será un identificador de tipo cadena que indicará el comando remoto que queremos ejecutar.
2. ```params```: puede no estar presente, puede ser un atributo, puede ser un objeto (incluso vacío) o puede ser una colección (incluso vacía), en función del tipo de comando elegido.

Para optimizar el canal de comunicación, el objeto JSON siempre debe enviarse minificado hacia el Hub, es decir, sin espacios en blanco.

## Respuestas (Response)

Las repuestas se reciben en un objeto JSON con 2 atributos:

1. ```response```: siempre será un identificador de tipo cadena que indicará el comando remoto al cual corresponde la respuesta.
2. ```data```: puede no estar presente, puede ser un atributo, puede ser un objeto (incluso vacío) o puede ser una colección (incluso vacía), en función del tipo de comando elegido.

```json
{
    "response": ""
    "data": {
        ...
    }
}
```


Para optimizar el canal de comunicación, el objeto JSON de respuesta siempre se devolverá minificado desde el Hub, es decir, sin espacios en blanco.

## Errores (Response: "error")

Los errores se reciben en un objeto JSON de tipo ```response``` cuyo valor específico será siempre ```"error"```. En el atributo ```data``` habrá una colección ```errors``` con la información detallada de cada error (pueden ser uno o varios):

1. ```response: "error"```
2. ```data```: contiene una colección ```errors``` con un objeto por cada error, en orden según se han producido:
   * ```errors```
       * ```code```: el número que identifica unívocamente el tipo de error.
       * ```message```: el texto descriptivo del error en el idioma en el que se esté trabajando.
       * ```info```: información adicional sobre el error (no siempre estará presente o tendrá valor).

Para optimizar el canal de comunicación, el objeto JSON de error siempre se devolverá minificado desde el Hub, es decir, sin espacios en blanco.

# get-tags

Consultar los Tags que se han asociado con el Hub en algún momento.

Los devuelve ordenados decendentemente por fecha de última conexión del Tag al Hub.

**PETICIÓN**
```json
{
    "command": "get-tags",
    "params": {
        "with_info": false,
        "only_with_changes": true
    }
}
```
* **with_info**: [true | **false**] Recibir un array de objetos con la información estadística resumida de cada Tag almacenada en el Hub.
* **only_with_changes**: [**true** | false] Devolver solo los tags que han recibido nuevas lecturas de sensores desde la última vez que lo consulté.

**RESPUESTA**
```json
{
    "response": "get-tags",
    "data": {
        "tag_list": [1, 2, 3, 4, 5, 6]
    }
}
```

**POSIBLES ERRORES**

* 101 - Error de memoria.
* 102 - Error de comunicación.
* 102 - Error inesperado.

# get-tag
Consultar la configuración y situación COMPLETA de un Tag

**PETICIÓN**
```json
{
    "command": "get-tag",
    "params": {
        "tag_id": 1,
        "with_config": true,
        "with_status": true,
        "with_calibration": false,
        "with_zones": true,
        "with_reads": false
    }
}
```
* **tag_id**: El ID del tag del que queremos recuperar la información.
* **with_config**: [**true** | false] Devolver o no el objeto ```config```del Tag, que contiene la última configuración conocida del Tag, incluyendo la de las ```custom_zones```.
* **with_status**: [**true** | false] Devolver o no el objeto ```status```del Tag, que contiene el último estado conocido del Tag.
* **with_calibration**: [true | **false**] Devolver o no el objeto ```calibration```del Tag, que contiene la calibración de fábrica del Tag.
* **with_zones**: [**true** | false] Devolver o no el objeto ```zones```del Tag, que contiene la información calculada para las ```custom_zones``` configuradas, siempre en función de las lecturas almacenadas.
* **with_reads**: [true | **false**] Devolver o no el objeto ```reads```del Tag, que contiene la última colección completa de lecturas que el Tag envió al Hub.

**RESPUESTA**
```json
{
    "response": "get-tag",
    "data": {
        "info": {
            "tag_id": 1,
            "fw_id": 1,
            "created_at": "2023-09-20 08:00:51",
            "updated_at": "2023-09-20 08:00:51",
            "first_read": "2023-09-20 08:00:51",
            "last_read": "2023-09-20 08:10:51",
            "last_records": 2,
            "total_records": 2,
            "total_reads": 1,
            "stored_days": 1,
            "stored_reads": 1
        },
        "config": {
            "tag_id": 1,
            "config_active": false,
            "config_unixtime": 0,
            "config_datetime": "",
            "config_id": 0,
            "custom_tag_id": 0,
            "custom_code": "",
            "custom_group_id": 0,
            "custom_location_id": 0,
            "custom_zones": [
                {
                    "id": 1,
                    "type": "temperature",
                    "type_id": 0,
                    "active": true,
                    "upper_limit": 15,
                    "lower_limit": -15
                },
                {
                    "id": 2,
                    "type": "humidity",
                    "type_id": 1,
                    "active": true,
                    "upper_limit": 85,
                    "lower_limit": 0
                }
            ],
            "rtc_timezone": "CET-1CEST,M3.5.0,M10.5.0/3",
            "rtc_sntp_active": true,
            "rtc_sntp_server": "pool.ntp.org",
            "read_mode": "interval",
            "read_interval": 300,
            "read_daily": "",
            "read_weekly": [
                "",
                "",
                "",
                "",
                "",
                "",
                ""
            ],
            "read_delay_active": false,
            "read_delay_seconds": 0,
            "read_delay_unixtime": 0,
            "read_delay_datetime": "",
            "search_ap_mode": "seconds",
            "search_ap_with_nfc": true,
            "search_ap_seconds": 86400,
            "search_ap_unixtime": 0,
            "search_ap_datetime": "",
            "search_ap_daily": "",
            "search_ap_weekly": [
                "",
                "",
                "",
                "",
                "",
                "",
                ""
            ],
            "search_ap_attempts_window": 60,
            "search_ap_attempts_interval": 2,
            "connection_method": "fast_wifi",
            "fast_ap_id": 0,
            "fast_ap_channel": 11,
            "fast_ap_address": "68:b6:b3:47:3e:2c",
            "fast_ap_pmk": "1234567890",
            "fast_ap_lmk": "1234567890",
            "wifi_dhcp_active": true,
            "wifi_ip": "",
            "wifi_mask": "",
            "wifi_gw": "",
            "wifi_dns_main": "",
            "wifi_dns_backup": "",
            "wifi_ssid": "MiWiFiAP",
            "wifi_pass": "MiPassword",
            "wifi_ap_hidden": false,
            "wifi_ap_ip": "192.168.29.1",
            "wifi_ap_mask": "255.255.255.0",
            "wifi_ap_ssid": "SingularTag-1",
            "wifi_ap_pass": "password",
            "custom_fields": [
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0
            ]
        },
        "status": {
            "tag_id": 1,
            "tag_revision_id": 0,
            "tag_firmware_version": "1",
            "tag_model": "SingularTag",
            "tag_address": "68:b6:b3:4f:f3:40",
            "tag_manufacturing_unixtime": 0,
            "tag_manufacturing_datetime": "",
            "tag_revision_unixtime": 0,
            "tag_revision_datetime": "",
            "tag_registered": true,
            "tag_registration_unixtime": 0,
            "tag_registration_datetime": "",
            "custom_tag_id": 0,
            "custom_code": "",
            "custom_group_id": 0,
            "custom_location_id": 0,
            "hw_certified": false,
            "hw_cert_datetime": "",
            "hw_cert_expiration_datetime": "",
            "hw_space_available": 0,
            "hw_cpu_temperature": 41.9,
            "hw_battery_voltage": 0,
            "hw_battery_level": 0,
            "hw_battery_maximum_life": 0,
            "hw_battery_estimated_life": 0,
            "rtc_sync_method": 2,
            "rtc_sync_unixtime": 1695195277,
            "rtc_sync_datetime": "2023-09-20 09:34:37",
            "rtc_current_unixtime": 1695197998,
            "rtc_current_datetime": "2023-09-20 10:19:58",
            "network_connected": true,
            "network_connection_duration": 3684,
            "real_ip": "192.168.0.237",
            "real_mask": "255.255.255.0",
            "real_gw": "192.168.0.1",
            "real_dns_main": "0.0.0.0",
            "real_dns_backup": "0.0.0.0",
            "current_ambience": {
                "temperature": 999,
                "humidity": 0,
                "pressure": 0
            },
            "last_read_duration": 1,
            "total_sessions": 1,
            "total_seconds_alive": 0,
            "total_records": 0,
            "total_zones": {
                "temperature": 1,
                "humidity": 2,
                "pressure": 0
            },
            "total_alarms": 0,
            "total_alarms_per_type": {
                "temperature": 0,
                "humidity": 0,
                "pressure": 0
            },
            "total_errors": 0,
            "errors": []
        },
        "calibration": {
            "tag_id": 1,
            "tag_revision_id": 0,
            "tag_firmware_version": "1",
            "tag_model": "SingularTag",
            "custom_tag_id": 0,
            "custom_code": "",
            "custom_group_id": 0,
            "custom_location_id": 0,
            "id": 0,
            "calibrated": false,
            "counter": 0,
            "calibration_unixtime": 0,
            "calibration_datetime": "",
            "expiration_unixtime": 0,
            "expiration_datetime": "",
            "samples": [
                {
                    "type": "temperature",
                    "reference": 0,
                    "value": 0,
                    "error": 0,
                    "pass": false
                },
                {
                    "type": "temperature",
                    "reference": 0,
                    "value": 0,
                    "error": 0,
                    "pass": false
                },
                {
                    "type": "humidity",
                    "reference": 0,
                    "value": 0,
                    "error": 0,
                    "pass": false
                },
                {
                    "type": "humidity",
                    "reference": 0,
                    "value": 0,
                    "error": 0,
                    "pass": false
                },
                {
                    "type": "pressure",
                    "reference": 0,
                    "value": 0,
                    "error": 0,
                    "pass": false
                },
                {
                    "type": "pressure",
                    "reference": 0,
                    "value": 0,
                    "error": 0,
                    "pass": false
                }
            ]
        },
        "zones": [
            {
                "id": 0,
                "type": "temperature",
                "type_id": 0,
                "active": true,
                "upper_limit": 85,
                "lower_limit": -40,
                "total_counter": 0,
                "upper_counter": 0,
                "lower_counter": 0,
                "time_counter": 0,
                "activity_percent": 0,
                "first": 0,
                "first_unixtime": 0,
                "first_datetime": "",
                "last": 0,
                "last_unixtime": 0,
                "last_datetime": "",
                "min": 0,
                "min_unixtime": 0,
                "min_datetime": "",
                "max": 0,
                "max_unixtime": 0,
                "max_datetime": "",
                "avg": 0,
                "mkt": 0,
                "sequential_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                },
                "total_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                }
            },
            {
                "id": 1,
                "type": "temperature",
                "type_id": 0,
                "active": true,
                "upper_limit": 15,
                "lower_limit": -15,
                "total_counter": 0,
                "upper_counter": 0,
                "lower_counter": 0,
                "time_counter": 0,
                "activity_percent": 0,
                "first": 0,
                "first_unixtime": 0,
                "first_datetime": "",
                "last": 0,
                "last_unixtime": 0,
                "last_datetime": "",
                "min": 0,
                "min_unixtime": 0,
                "min_datetime": "",
                "max": 0,
                "max_unixtime": 0,
                "max_datetime": "",
                "avg": 0,
                "mkt": 0,
                "sequential_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                },
                "total_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                }
            },
            {
                "id": 0,
                "type": "humidity",
                "type_id": 1,
                "active": true,
                "upper_limit": 100,
                "lower_limit": 0,
                "total_counter": 0,
                "upper_counter": 0,
                "lower_counter": 0,
                "time_counter": 0,
                "activity_percent": 0,
                "first": 0,
                "first_unixtime": 0,
                "first_datetime": "",
                "last": 0,
                "last_unixtime": 0,
                "last_datetime": "",
                "min": 0,
                "min_unixtime": 0,
                "min_datetime": "",
                "max": 0,
                "max_unixtime": 0,
                "max_datetime": "",
                "avg": 0,
                "mkt": 0,
                "sequential_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                },
                "total_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                }
            },
            {
                "id": 1,
                "type": "humidity",
                "type_id": 1,
                "active": true,
                "upper_limit": 85,
                "lower_limit": 0,
                "total_counter": 0,
                "upper_counter": 0,
                "lower_counter": 0,
                "time_counter": 0,
                "activity_percent": 0,
                "first": 0,
                "first_unixtime": 0,
                "first_datetime": "",
                "last": 0,
                "last_unixtime": 0,
                "last_datetime": "",
                "min": 0,
                "min_unixtime": 0,
                "min_datetime": "",
                "max": 0,
                "max_unixtime": 0,
                "max_datetime": "",
                "avg": 0,
                "mkt": 0,
                "sequential_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                },
                "total_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                }
            },
            {
                "id": 2,
                "type": "humidity",
                "type_id": 1,
                "active": true,
                "upper_limit": 85,
                "lower_limit": 0,
                "total_counter": 0,
                "upper_counter": 0,
                "lower_counter": 0,
                "time_counter": 0,
                "activity_percent": 0,
                "first": 0,
                "first_unixtime": 0,
                "first_datetime": "",
                "last": 0,
                "last_unixtime": 0,
                "last_datetime": "",
                "min": 0,
                "min_unixtime": 0,
                "min_datetime": "",
                "max": 0,
                "max_unixtime": 0,
                "max_datetime": "",
                "avg": 0,
                "mkt": 0,
                "sequential_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                },
                "total_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                }
            },
            {
                "id": 0,
                "type": "pressure",
                "type_id": 2,
                "active": true,
                "upper_limit": 1100000,
                "lower_limit": 300,
                "total_counter": 0,
                "upper_counter": 0,
                "lower_counter": 0,
                "time_counter": 0,
                "activity_percent": 0,
                "first": 0,
                "first_unixtime": 0,
                "first_datetime": "",
                "last": 0,
                "last_unixtime": 0,
                "last_datetime": "",
                "min": 0,
                "min_unixtime": 0,
                "min_datetime": "",
                "max": 0,
                "max_unixtime": 0,
                "max_datetime": "",
                "avg": 0,
                "mkt": 0,
                "sequential_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                },
                "total_alarm": {
                    "active": false,
                    "triggered": false,
                    "threshold": 0,
                    "counter": 0,
                    "first_unixtime": 0,
                    "first_datetime": "",
                    "last_unixtime": 0,
                    "last_datetime": ""
                }
            }
        ],
        "reads": [
            {
                "u": 1695191400,
                "b": 100,
                "c": 32.5,
                "t": 12.32,
                "h": 95.55,
                "p": 975.35
            },
            {
                "u": 1695191700,
                "b": 99,
                "c": 31.5,
                "t": 12.8,
                "h": 94.82,
                "p": 975.39
            }
        ]
    }
}
```

**POSIBLES ERRORES**

* 101 - Error de memoria.
* 102 - Error de comunicación.
* 102 - Error inesperado.

# get-reads
Consultar las últimas lecturas que fueron leídas por los sensores del _SingularTag_ y que, en algún momento, fueron enviadas al Hub.

**PETICIÓN**
```json
{
    "command": "get-reads",
    "params": {
        "tag_id": 1,
        "filter": "new | last | unixtime | datetime",
        "from": "NUMBER | UNIXTIME | DATETIME",
        "to": "NUMBER | UNIXTIME | DATETIME"
    }
}
```
* **tag_id**: el ID del Tag del que queremos recuperar la información.
* **filter**:
  * **new**: devuelve las nuevas lecturas que se hayan realizado desde la última consulta marcada como correcta (ver #set-config). COMPORTAMIENTO POR DEFECTO.
  * **last**: devuelve las últimas lecturas realizadas desde la posición ```from``` pasada por parámetro.
  * **unixtime**: devuelte las lecturas entre dos fechas en formato _unixtime_ (segundos desde 1970) especificadas en ```from```y ```to```.
  * **datetime**: devuelte las lecturas entre dos fechas en formato _datetime_ (Y-m-d h:i:s) especificadas en ```from```y ```to```.

**RESPUESTA**
```json
{
    "response": "get-reads",
    "data": [
        "reads": [{
            "u": 1695191400,
            "b": 100,
            "c": 32.5,
            "t": 12.32,
            "h": 95.55,
            "p": 975.35
        },
        {
            "u": 1695191700,
            "b": 99,
            "c": 31.5,
            "t": 12.8,
            "h": 94.82,
            "p": 975.39
        }]
    ]
}
```
* **u**: Unixtime en que fue realizada la medición (lectura) de los sensores. No tiene que ver con la fecha en que esta información fue enviada desde el Tag al Hub.
* **b**: Porcentaje (de 0% a 100%) de batería del Tag en el momento de la lectura de los sensores.
* **c**: Temperatura (en grados centígrados, ºC) de la CPU del Tag en el momento de la lectura de los sensores.
* **t**: Temperatura (en grados centígrados, ºC) del sensor de temperatura del ambiente. Redondeada a dos decimales.
* **h**: Humedad (de 0% a 100%) del sensor de humedad relativa del aire del ambiente. Redondeada a dos decimales.
* **p**: Presion atmosférica (en hecto pascales, hP) del sensor de presión barométrica del ambiente. Redondeada a dos decimales.
  
**POSIBLES ERRORES**

* 101 - Error de memoria.
* 102 - Error de comunicación.
* 102 - Error inesperado.


# set-config

Enviar una nueva configuración para que sea actualizada en uno o varios Tags.

Los Tags no se configuran en tiempo real, sino que se intentan configurar la próxima vez que se conecten al Hub, siempre y cuando se cumpla la condición de fecha de actualización.

Permite configurar de una vez todas las zonas personalizadas (```custom_zones```).

Si se envía un parámetro que no puede ser configurado, como ```tag_id```, el Tag lo ignorará.

**PETICIÓN**
```json
{
    "command": "set-config",
    "params": {
        "tags": [1, 2, 3],
        "active_from": "2023-10-01 00:00:00",
        "config": {
            "config_active": false,
            "config_id": 0,
            "custom_tag_id": 0,
            "custom_code": "",
            "custom_group_id": 0,
            "custom_location_id": 0,
            "custom_zones": [
                {
                    "id": 1,
                    "type": "temperature",
                    "type_id": 0,
                    "active": true,
                    "upper_limit": 15,
                    "lower_limit": -15
                },
                {
                    "id": 2,
                    "type": "humidity",
                    "type_id": 1,
                    "active": true,
                    "upper_limit": 85,
                    "lower_limit": 0
                }
            ],
            "rtc_timezone": "CET-1CEST,M3.5.0,M10.5.0/3",
            "rtc_sntp_active": true,
            "rtc_sntp_server": "pool.ntp.org",
            "read_mode": "interval",
            "read_interval": 300,
            "read_daily": "",
            "read_weekly": [
                "",
                "",
                "",
                "",
                "",
                "",
                ""
            ],
            "read_delay_active": false,
            "read_delay_seconds": 0,
            "read_delay_unixtime": 0,
            "read_delay_datetime": "",
            "search_ap_mode": "seconds",
            "search_ap_with_nfc": true,
            "search_ap_seconds": 86400,
            "search_ap_unixtime": 0,
            "search_ap_datetime": "",
            "search_ap_daily": "",
            "search_ap_weekly": [
                "",
                "",
                "",
                "",
                "",
                "",
                ""
            ],
            "search_ap_attempts_window": 60,
            "search_ap_attempts_interval": 2,
            "connection_method": "fast_wifi",
            "fast_ap_id": 0,
            "fast_ap_channel": 11,
            "fast_ap_address": "68:b6:b3:47:3e:2c",
            "fast_ap_pmk": "1234567890",
            "fast_ap_lmk": "1234567890",
            "wifi_dhcp_active": true,
            "wifi_ip": "",
            "wifi_mask": "",
            "wifi_gw": "",
            "wifi_dns_main": "",
            "wifi_dns_backup": "",
            "wifi_ssid": "MiWiFiAP",
            "wifi_pass": "MiPassword",
            "wifi_ap_hidden": false,
            "wifi_ap_ip": "192.168.29.1",
            "wifi_ap_mask": "255.255.255.0",
            "wifi_ap_ssid": "SingularTag-1",
            "wifi_ap_pass": "password",
            "custom_fields": [
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0
            ]
        }
    }
}
```
* **tags**: lista de IDs de Tags que queremos configurar. Si se envía vacía, entonces la configuración se aplicará sobre cualquier Tag que se conecte al Hub.
* **active_from**: fecha desde la cual debe activarse esta configuración. Si se envía vacío o no se envía, la configuración se aplicará tan pronto haya comunicación con los Tags indicados en la colección ```tags```.
* **config**: configuración que queremos aplicar sobre los Tags indicados en la colección ```tags```.

**RESPUESTA**

* Dentro de la colección ```tags``` se indican los Tags para los que se ha almacenado correctamente la configuración.
* Dentro del objeto ```config``` se indican los campos que sí podrán ser actualizados.

```json
{
    "response": "set-config",
    "data": [
        "tags": [1, 2, 3],
        "active_from": "2023-10-01 00:00:00",
        "config": {
            ...
        }
    ]
}
```

**POSIBLES ERRORES**

* 101 - Error de memoria.
* 102 - Error de comunicación.
* 102 - Error inesperado.


# channel
Listado de canales de tiempo real a los que me puedo suscribir y a los que estoy suscrito.

**PETICIÓN**
```json
{
    "command": "channels"
    "params": {}
}
```
* **Canales disponibles**:
  * ```tag```: Devuelve información en tiempo real cada vez que un Tag se conecta al Hub y le envía culquier tipo de información. Es igual que hacer una petición de tipo {get-tag}, pero sin ```calibration```y sin ```reads```.
  * ```reads```: Devuelve información en tiempo real cada vez que un Tag se conecta al Hub y le envía información sobre sus lecturas. Es igual que hacer una petición de tipo {get-reads}.
  * ```config```: Devuelve información en tiempo real cada vez que se actualiza la configuración de un Tag porque este se ha conectado al Hub y tenía programada una configuración.
  * ```errors```: Devuelve información en tiempo real sobre los errores que se pueden estar produciendo en el Hub a medida que recibe conexiones de los Tags.


**RESPUESTA**
```json
{
    "response": "channels",
    "data": [{
        "channel": "reads",
        "subscribed": false,
        "tags": []
    }]
}
```

**POSIBLES ERRORES**

* 101 - Error de memoria.
* 102 - Error de comunicación.
* 102 - Error inesperado.

# subscribe

Suscribirme a un canal de tiempo real.

**PETICIÓN**
```json
{
    "command": "subscribe",
    "params": {
        "channel": "reads",
        "tags": []
    }
}
```
* **channel**: Canal al que deseas suscribirte.
* **tags**: lista de tags de los que quieres recibir información en este canal de tiempo real (reemplaza el listado de tags vigente). Vacio para recuperar información de todos los Tags.

**RESPUESTA**
```json
{
    "response": "subscribe",
    "data": {
        "channel": "reads",
        "subscribed": true,
        "tags": []
    }
}
```

**POSIBLES ERRORES**

* 101 - Error de memoria.
* 102 - Error de comunicación.
* 102 - Error inesperado.


# unsubscribe

Desuscribirme de un canal de tiempo real.

**PETICIÓN**
```json
{
    "command": "unsubscribe",
    "params": {
        "channel": "reads"
    }
}
```
* **channel**: Canal del que deseas desuscribirte.

**RESPUESTA**
```json
{
    "response": "unsubscribe",
    "data": {
        "channel": "reads",
        "subscribed": false
    }
}
```

**POSIBLES ERRORES**

* 101 - Error de memoria.
* 102 - Error de comunicación.
* 102 - Error inesperado.
