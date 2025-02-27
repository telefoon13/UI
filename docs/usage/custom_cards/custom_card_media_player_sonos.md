---
title: card_media_player_sonos_with_controls
hide:
  - toc
---
<!-- markdownlint-disable MD046 -->

### Media player: sonos

> NOTE
> This card is under review and is not ready to use!

![Sonos](../../assets/img/media_player_sonos.png)

<details>
<summary>Usage</summary>

#### Example

```yaml
- type: 'custom:button-card'
  template: card_media_player_sonos_with_controls
  variables:
    ulm_card_media_player_with_controls_name: "Slaapkamer"
    ulm_card_media_player_with_controls_entity: media_player.slaapkamer
```

#### Variables

<table>
<tr>
<th>Variable</th>
<th>Example</th>
<th>Required</th>
<th>Explanation</th>
</tr>
<tr>
<td>ulm_card_media_player_with_controls_name</td>
<td>Sonos room 1</td>
<td>Yes</td>
<td>Name shown in lovelace</td>
<tr>
<td>ulm_card_media_player_with_controls_entity</td>
<td>media_player.sonos_room_1</td>
<td>Yes</td>
<td>Entity id</td>
</tr>
</table>
<br />
</details>

<details>
<summary>Template code</summary>

```yaml
card_media_player_sonos:
  size: "20px"
  show_icon: true
  show_entity_picture: false
  show_state: false
  show_name: true
  template:
    - "green_playing"
    - "icon_info_bg"
    - "ulm_language_variables"
  label: |
    [[[
        if (entity.state == 'idle'){
          return variables.ulm_off;
        }
        if (entity.state == 'paused'){
          return variables.ulm_paused;
        }
        if (entity.state == 'unavailable'){
          return variables.ulm_unavailable;
        } else {
        return (entity.attributes.source) + ' • ' +  ( Math.round(entity.attributes.volume_level / 0.01)) + '%' ;
        }
    ]]]

card_media_player_sonos_with_controls:
  variables:
    ulm_card_media_player_with_controls_name: "No name set"
  triggers_update:
    - "[[[ ulm_card_media_player_with_controls_entity ]]]"
  styles:
    grid:
      - grid-template-areas: '"item1" "item2"'
      - grid-template-columns: "1fr"
      - grid-template-rows: "min-content  min-content"
      - row-gap: "12px"
    card:
      - border-radius: "var(--border-radius)"
      - box-shadow: "var(--box-shadow)"
      - padding: "12px"
  custom_fields:
    item1:
      card:
        type: "custom:button-card"
        template:
          - "ulm_language_variables"
          - "card_media_player_sonos"
        tap_action:
          action: "more-info"
        entity: "[[[ return variables.ulm_card_media_player_with_controls_entity ]]]"
        name: "[[[ return variables.ulm_card_media_player_with_controls_name ]]]"
        styles:
          card:
            - box-shadow: "none"
            - padding: "0px"
    item2:
      card:
        type: "custom:button-card"
        template: "list_items"
        custom_fields:
          item1:
            card:
              type: "custom:button-card"
              template: "widget_icon"
              tap_action:
                action: "call-service"
                service: "media_player.volume_down"
                service_data:
                  entity_id: "[[[ return variables.ulm_card_media_player_with_controls_entity ]]]"
              icon: "mdi:volume-minus"
          item2:
            card:
              type: "custom:button-card"
              template: "widget_icon"
              entity: "[[[ return variables.ulm_card_media_player_with_controls_entity ]]]"
              tap_action:
                action: "call-service"
                service: "media_player.media_play_pause"
                service_data:
                  entity_id: "[[[ return variables.ulm_card_media_player_with_controls_entity ]]]"
              icon: "mdi:pause"
              state:
                - value: "paused"
                  icon: "mdi:play"
                - value: "off"
                  icon: "mdi:play"
          item3:
            card:
              type: "custom:button-card"
              template: "widget_icon"
              tap_action:
                action: "call-service"
                service: "media_player.volume_up"
                service_data:
                  entity_id: "[[[ return variables.ulm_card_media_player_with_controls_entity ]]]"
              icon: "mdi:volume-plus"

icon_info_bg_sonos:
  color: "var(--google-grey)"
  show_icon: true
  show_label: true
  show_name: true
  state:
    - value: "unavailable"
      styles:
        custom_fields:
          notification:
            - border-radius: "50%"
            - position: "absolute"
            - left: "3px"
            - top: "8px"
            - height: "18px"
            - width: "18px"
            - font-size: "12px"
            - line-height: "14px"
            - background-color: >
                [[[
                  return "rgba(var(--color-red),1)";
                ]]]
  styles:
    icon:
      - color: "rgba(var(--color-theme),0.2)"
    label:
      - justify-self: "start"
      - align-self: "start"
      - font-weight: "bold"
      - font-size: "12px"
      - filter: "opacity(40%)"
      - margin-left: "12px"
    name:
      - align-self: "end"
      - justify-self: "start"
      - font-weight: "bold"
      - font-size: "14px"
      - margin-left: "12px"
    state:
      - justify-self: "start"
      - align-self: "start"
      - font-weight: "bold"
      - font-size: "12px"
      - filter: "opacity(40%)"
      - margin-left: "12px"
    img_cell:
      - background-color: "rgba(var(--color-theme),0.05)"
      - border-radius: "50%"
      - place-self: "center"
    grid:
      - grid-template-areas: "'i n' 'i l'"
      - grid-template-columns: "min-content auto"
      - grid-template-rows: "min-content min-content"
    card:
      - border-radius: "var(--border-radius)"
      - box-shadow: "var(--box-shadow)"
      - padding: "12px"
  size: "20px"

green_playing:
  state:
    - styles:
        icon:
          - color: "rgba(var(--color-green),1)"
        img_cell:
          - background-color: "rgba(var(--color-green), 0.2)"
      value: "playing"
```

</details>
