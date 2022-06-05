![hacs_badge](https://img.shields.io/badge/hacs-custom-orange.svg) [![BuyMeCoffee][buymecoffeebedge]][buymecoffee]

# Drivvo Sensor Component

![logo.jpg](logo.png)

Custom component to put data from [drivvo.com](https://www.drivvo.com/pt) for use as sensors in Home Assistant.

# Installation Instructions

## HACS

- Utilizing [HACS](https://hacs.xyz/) will allow for easy updates to the custom component.
- Head to the HACS store, click the ellipse and select custom repositories, add https://github.com/theglus/sensor.drivvo as a new Integration.
- Click `Explore & Add repositiories` to Install the `Drivvo` integration.
- Restart Home-Assistant.

## Manual

- Copy the `custom_components/drivvo` to your directory `<config dir>/custom_components`.
- Configure.
- Restart Home-Assistant.

# Configuration

```yaml
- platform: drivvo
  email: your-email
  password: your-password
  model: model-vehicle ## Enter the car model name, for exemple: Jeep Renegade. 
  id_vehicle: id-vehicle
```

## Retrieve your vehicle_id

Open [login](https://web.drivvo.com/auth/login), launch your browser console using the following shortcut *(CTRL + SHIFT + J)*, select the **Network** tab and choose **XHR** as shown in the image below:

![example 1](example1.png)

Login to Drivvo, your console will show a list of network requests, select the one with the prefix **web** as show in the image below:

![example 1](example2.png)

Select the request, and in the **Headers** section of the request the accessed url will appear, which will follow this pattern:: https://api.drivvo.com/veiculo/12345/abastecimento/web

The id of your vehicle will be the number that will be in place of 12345, use it in your Drivvo configuration, adding it in the parameter **id_vehicle**.

# Building a Lovelace Card

To view your Drivvo information, here is an example of a Lovelace card. Remember to replace the entities on the card with your entities.


```yaml
type: custom:config-template-card
entities:
  - sensor.jeep_renegade_abastecimento # Replace with your sensor
card:
  type: entities
  show_header_toggle: 'off'
  style: |
    .card-header {
      padding: 0px 0px 0px 0px !important;
    }
  entities:
    - type: custom:hui-vertical-stack-card
      cards:
        - type: horizontal-stack
          cards:
            - type: picture
              style: |
                ha-card {
                    --paper-card-background-color: 'rgba(0, 0, 0, 0.0)';
                    --ha-card-background: "rgba(0, 0, 0, 0.0)";
                    --ha-card-box-shadow: 'none';
                }
              image: /local/images/jeep.png # Replace with your vehicle's image
            - type: custom:button-card
              layout: icon_name_state2nd
              show_icon: true
              show_state: true
              styles:
                grid:
                  - grid-template-columns: 50px auto
                icon:
                  - padding: 0px 0px
                  - height: 100px
                  - width: 30px
                card:
                  - '--ha-card-background': rgba(0, 0, 0, 0.0)
                  - '--ha-card-box-shadow': none
                state:
                  - padding: 0px 10px
                  - justify-self: start
                  - font-family: Roboto, sans-serif
                  - font-size: 15px
                name:
                  - padding: 0px 10px
                  - justify-self: start
                  - color: var(--secondary-text-color)
              entity: sensor.jeep_renegade_abastecimento # Replace with your sensor
              name: Abastecimentos
              icon: mdi:car
        - type: custom:bar-card
          show_icon: true
          align: split
          columns: 1
          max: 41
          positions:
            icon: inside
            indicator: inside
            name: inside
            value: inside
          unit_of_measurement: Litros
          animation: 'on'
          severity:
            - color: '#fd0000'
              from: 1
              to: 19
            - color: '#ffaa00'
              from: 20
              to: 29
            - color: '#2CE026'
              from: 30
              to: 41
          style: |
            ha-card {
                --paper-card-background-color: 'rgba(0, 0, 0, 0.0)';
                --ha-card-background: "rgba(0, 0, 0, 0.0)";
                --paper-item-icon-color: 'var(--text-primary-color';
                --ha-card-box-shadow: 'none';
            }
          entities:
            - entity: sensor.jeep_renegade_abastecimento # Replace with your sensor
              attribute: volume_de_combustivel
          name: Volume de combustível
          entity_row: true
        - type: sensor
          entity: sensor.jeep_renegade_abastecimento # Replace with your sensor
          attribute: odometro
          graph: line
          detail: 2
          name: Odômetro
          icon: mdi:car-cruise-control
          unit: km
        - type: sensor
          entity: sensor.jeep_renegade_abastecimento # Replace with your sensor
          attribute: preco_do_combustivel
          graph: line
          detail: 2
          name: Preço atual da gasolina
          icon: mdi:cash
          unit: R$
        - type: sensor
          entity: sensor.jeep_renegade_abastecimento # Replace with your sensor
          attribute: valor_total_pago
          graph: line
          detail: 2
          name: Valor total pago
          icon: mdi:cash
          unit: R$
        - type: sensor
          entity: sensor.jeep_renegade_abastecimento # Replace with your sensor
          attribute: soma_total_de_valores_pagos_em_todos_os_abastecimentos
          graph: line
          detail: 2
          name: Total pago em todos os abestecimentos até então
          icon: mdi:cash
          unit: R$
        - type: sensor
          entity: sensor.jeep_renegade_abastecimento # Replace with your sensor
          attribute: km_percorridos_desde_o_ultimo_abastecimento
          graph: line
          detail: 2
          name: Kms percorridos desde o último abastecimento
          icon: mdi:car-hatchback
          unit: km
```

After configuration, the card above will look like this:

![example 1](example3.png)

# Debugging

```yaml
logger:
  default: info
  logs:
    custom_components.drivvo: debug
```

[buymecoffee]: https://www.buymeacoffee.com/hudsonbrendon
[buymecoffeebedge]: https://camo.githubusercontent.com/cd005dca0ef55d7725912ec03a936d3a7c8de5b5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6275792532306d6525323061253230636f666665652d646f6e6174652d79656c6c6f772e737667