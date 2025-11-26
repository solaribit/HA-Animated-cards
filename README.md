# HA-Animated-cards

![ezgif-8e9c11636a22c613](https://github.com/user-attachments/assets/4f5a1af0-d89b-4ed9-9c8c-36e678045580)


<details>
  <summary><strong>1 - smartplug</summary>

  ```yaml
type: custom:mushroom-entity-card
entity: switch.plug_6_local
tap_action:
  action: toggle
icon: mdi:power-plug
icon_color: green
name: Smart Plug
card_mod:
	  style:
	    mushroom-shape-icon$: |
	      .shape {
	        {# ========== USER CONFIG ========== #}
	        {# true = number mode, false = state mode #}
	        {% set use_number      = false %}
	
	        {# STATE MODE SETTINGS #}
	        {% set state_entity    = 'switch.plug_6_local' %}
	        {% set active_value    = 'on' %}
	
	        {# OPTIONAL: NUMBER MODE SETTINGS #}
	        {% set number_entity   = 'sensor.plug_power' %}
	        
	        {# '>' '<' '=' '>=' '<=' #}
	        {% set number_operator = '>' %}
	        
	        {% set threshold       = 0.0 %}
	        {# ========== END USER CONFIG ====== #}
	
	        {# ======== TRIGGER DECISION LOGIC ======== #}
	        {% if use_number %}
	          {% set num = states(number_entity) | float(0) %}
	          
	          {# Evaluate numeric rule #}
	          {% if number_operator == '>' %}
	            {% set trigger_active = (num > threshold) %}
	          {% elif number_operator == '<' %}
	            {% set trigger_active = (num < threshold) %}
	          {% elif number_operator == '=' %}
	            {% set trigger_active = (num == threshold) %}
	          {% elif number_operator == '>=' %}
	            {% set trigger_active = (num >= threshold) %}
	          {% elif number_operator == '<=' %}
	            {% set trigger_active = (num <= threshold) %}
	          {% else %}
	            {% set trigger_active = false %}
	          {% endif %}
	
	        {% else %}
	          {# State mode #}
	          {% if states(state_entity) == active_value %}
	            {% set trigger_active = true %}
	          {% else %}
	            {% set trigger_active = false %}
	          {% endif %}
	        {% endif %}
	        {# =========== END TRIGGER LOGIC =========== #}
	
	        {% if trigger_active %}
	          --shape-animation: ring-breathe 1.8s ease-out infinite;
	          opacity: 1;
	        {% else %}
	          --shape-animation: none;
	          opacity: 0.7;
	        {% endif %}
	      }
	
	      @keyframes ring-breathe {
	        0%   { box-shadow: 0 0 0 0 rgba(var(--rgb-{{ config.icon_color }}), 0.9); }
	        70%  { box-shadow: 0 0 0 14px rgba(var(--rgb-{{ config.icon_color }}), 0.0); }
	        100% { box-shadow: 0 0 0 0 rgba(var(--rgb-{{ config.icon_color }}), 0.0); }
	      }
	    .: |
	      mushroom-shape-icon {
	        --icon-size: 65px;
	        display: flex;
	        margin: -18px 0 10px -20px !important;
	        padding-right: 10px;
	      }
	      ha-card {
	        clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
	      }

  ```
</details>

<details>
  <summary><strong>2 - Charger</summary>

  ```yaml
type: custom:mushroom-entity-card
entity: switch.plug_6_local
tap_action:
  action: toggle
icon: mdi:power-socket-au
icon_color: green
name: Charger
card_mod:
	  style:
	    mushroom-shape-icon$: |
	      .shape {
	        {# ========== USER CONFIG ========== #}
	        {# true = number mode, false = state mode #}
	        {% set use_number      = false %}
	
	        {# STATE MODE SETTINGS #}
	        {% set state_entity    = 'switch.plug_6_local' %}
	        {% set active_value    = 'on' %}
	
	        {# OPTIONAL: NUMBER MODE SETTINGS #}
	        {% set number_entity   = 'sensor.plug_power' %}
	        
	        {# '>' '<' '=' '>=' '<=' #}
	        {% set number_operator = '>' %}
	        
	        {% set threshold       = 0.0 %}
	        {# ========== END USER CONFIG ====== #}
	
	        {# ======== TRIGGER DECISION LOGIC ======== #}
	        {% if use_number %}
	          {% set num = states(number_entity) | float(0) %}
	
	          {% if number_operator == '>' %}
	            {% set trigger_active = (num > threshold) %}
	          {% elif number_operator == '<' %}
	            {% set trigger_active = (num < threshold) %}
	          {% elif number_operator == '=' %}
	            {% set trigger_active = (num == threshold) %}
	          {% elif number_operator == '>=' %}
	            {% set trigger_active = (num >= threshold) %}
	          {% elif number_operator == '<=' %}
	            {% set trigger_active = (num <= threshold) %}
	          {% else %}
	            {% set trigger_active = false %}
	          {% endif %}
	        {% else %}
	          {% set trigger_active = (states(state_entity) == active_value) %}
	        {% endif %}
	        {# =========== END TRIGGER LOGIC =========== #}
	
	        {% if trigger_active %}
	          {# adjust speed based on this plug's own power attribute #}
	          {% set p = state_attr(config.entity, 'current_power_w') | float(0) %}
	          {% set dur = 1.6 - (p / 800) %}
	          --shape-animation: ultra-plug-on {{ [0.4, dur] | max | round(2) }}s ease-in-out infinite;
	          --plug-glow-animation: ultra-plug-glow 1.8s ease-in-out infinite;
	          --plug-arcs-animation: ultra-plug-arcs 1.2s linear infinite;
	          opacity: 1;
	        {% else %}
	          --shape-animation: none;
	          --plug-glow-animation: none;
	          --plug-arcs-animation: none;
	          opacity: 0.7;
	        {% endif %}
	
	        transform-origin: 50% 50%;
	        position: relative;
	      }
	
	      .shape::before,
	      .shape::after {
	        content: '';
	        position: absolute;
	        inset: -4px;
	        border-radius: inherit;
	        pointer-events: none;
	      }
	
	      .shape::before {
	        animation: var(--plug-glow-animation);
	      }
	
	      .shape::after {
	        animation: var(--plug-arcs-animation);
	      }
	
	      @keyframes ultra-plug-on {
	        0%   { transform: scale(1); }
	        25%  { transform: scale(1.05) translateY(-1px); }
	        50%  { transform: scale(1.08) translateY(-2px); }
	        75%  { transform: scale(1.04) translateY(-1px); }
	        100% { transform: scale(1); }
	      }
	
	      @keyframes ultra-plug-glow {
	        0% {
	          box-shadow:
	            0 0 10px 3px rgba(var(--rgb-{{ config.icon_color }}), 0.6),
	            0 0 20px 8px rgba(var(--rgb-{{ config.icon_color }}), 0.3);
	        }
	        50% {
	          box-shadow:
	            0 0 18px 6px rgba(var(--rgb-{{ config.icon_color }}), 0.9),
	            0 0 32px 12px rgba(var(--rgb-{{ config.icon_color }}), 0.4);
	        }
	        100% {
	          box-shadow:
	            0 0 10px 3px rgba(var(--rgb-{{ config.icon_color }}), 0.6),
	            0 0 20px 8px rgba(var(--rgb-{{ config.icon_color }}), 0.3);
	        }
	      }
	
	      @keyframes ultra-plug-arcs {
	        0% {
	          box-shadow:
	            -10px -6px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.0),
	            12px 4px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.0);
	        }
	        25% {
	          box-shadow:
	            -10px -6px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.7),
	            12px 4px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.4);
	        }
	        50% {
	          box-shadow:
	            -6px 2px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.3),
	            10px -8px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.7);
	        }
	        75% {
	          box-shadow:
	            -12px 4px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.5),
	            8px 0 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.2);
	        }
	        100% {
	          box-shadow:
	            -10px -6px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.0),
	            12px 4px 0 -4px rgba(var(--rgb-{{ config.icon_color }}), 0.0);
	        }
	      }
	    .: |
	      mushroom-shape-icon {
	        --icon-size: 65px;
	        display: flex;
	        margin: -18px 0 10px -20px !important;
	        padding-right: 10px;
	      }
	      ha-card {
	        clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
	      }

  ```
</details>

<details>
  <summary><strong>3 - Fan</summary>

  ```yaml
type: custom:mushroom-entity-card
entity: switch.plug_6_local
tap_action:
  action: toggle
icon: mdi:fan
icon_color: blue
name: Fan
card_mod:
	  style:
	    mushroom-shape-icon$: |
	      .shape {
	        {# ========== USER CONFIG ========== #}
	        {# true = number mode, false = state mode #}
	        {% set use_number      = false %}
	
	        {# STATE MODE SETTINGS #}
	        {% set state_entity    = 'switch.plug_6_local' %}
	        {% set active_value    = 'on' %}
	
	        {# OPTIONAL: NUMBER MODE SETTINGS #}
	        {% set number_entity   = 'sensor.plug_power' %}
	        
	        {# '>' '<' '=' '>=' '<=' #}
	        {% set number_operator = '>' %}
	        
	        {% set threshold       = 0.0 %}
	        {# ========== END USER CONFIG ====== #}
	
	        {# ======== TRIGGER DECISION LOGIC ======== #}
	        {% if use_number %}
	          {% set num = states(number_entity) | float(0) %}
	          {% if number_operator == '>' %}
	            {% set trigger_active = (num > threshold) %}
	          {% elif number_operator == '<' %}
	            {% set trigger_active = (num < threshold) %}
	          {% elif number_operator == '=' %}
	            {% set trigger_active = (num == threshold) %}
	          {% elif number_operator == '>=' %}
	            {% set trigger_active = (num >= threshold) %}
	          {% elif number_operator == '<=' %}
	            {% set trigger_active = (num <= threshold) %}
	          {% else %}
	            {% set trigger_active = false %}
	          {% endif %}
	        {% else %}
	          {% set trigger_active = (states(state_entity) == active_value) %}
	        {% endif %}
	        {# =========== END TRIGGER LOGIC =========== #}
	
	        {% if trigger_active %}
	          {# Fan speed logic based on THIS card's entity #}
	          {% if is_state(config.entity, 'on') %}
	            --shape-animation: fan-turbine 0.5s linear infinite;
	            opacity: 1;
	          {% elif is_state(config.entity, 'medium') %}
	            --shape-animation: fan-turbine 0.8s linear infinite;
	            opacity: 1;
	          {% elif is_state(config.entity, 'low') %}
	            --shape-animation: fan-turbine 1.2s linear infinite;
	            opacity: 1;
	          {% else %}
	            --shape-animation: fan-rock 2.4s ease-in-out infinite;
	            opacity: 0.7;
	          {% endif %}
	        {% else %}
	          --shape-animation: none;
	          opacity: 0.7;
	        {% endif %}
	
	        transform-origin: 50% 50%;
	        position: relative;
	        box-shadow:
	          0 0 0 0 rgba(var(--rgb-{{ config.icon_color }}), 0.6),
	          0 0 0 6px rgba(var(--rgb-{{ config.icon_color }}), 0.08),
	          0 0 18px 0 rgba(var(--rgb-{{ config.icon_color }}), 0.7);
	      }
	
	      @keyframes fan-turbine {
	        0% {
	          transform: rotate(0deg) scale(1);
	          filter: drop-shadow(0 0 3px rgba(var(--rgb-{{ config.icon_color }}), 0.9));
	        }
	        25% {
	          transform: rotate(90deg) scale(1.02);
	        }
	        50% {
	          transform: rotate(180deg) scale(1.04);
	          filter: drop-shadow(0 0 8px rgba(var(--rgb-{{ config.icon_color }}), 1));
	        }
	        75% {
	          transform: rotate(270deg) scale(1.02);
	        }
	        100% {
	          transform: rotate(360deg) scale(1);
	          filter: drop-shadow(0 0 3px rgba(var(--rgb-{{ config.icon_color }}), 0.9));
	        }
	      }
	
	      @keyframes fan-rock {
	        0%   { transform: rotate(-4deg); }
	        50%  { transform: rotate(4deg); }
	        100% { transform: rotate(-4deg); }
	      }
	    .: |
	      mushroom-shape-icon {
	        --icon-size: 65px;
	        display: flex;
	        margin: -18px 0 10px -20px !important;
	        padding-right: 10px;
	      }
	      ha-card {
	        clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
	      }

  ```
</details>

<details>
  <summary><strong>4 - Fan</summary>

  ```yaml
type: custom:mushroom-fan-card
entity: fan.living_room_fan
show_percentage_control: true
show_oscillate_control: true
show_direction_control: true
collapsible_controls: false
fill_container: false
icon_animation: false
icon_type: icon
card_mod:
	  style:
	    mushroom-shape-icon$: |
	      .shape {
	        {# ========= COLOR CONFIG ========= #}
	        {# change to 'blue', 'red', 'purple', etc. #}
	        {% set fan_color = 'green' %}
	        --fan-color-rgb: var(--rgb-{{ fan_color }});
	        {# ================================ #}
	
	        {# ========== USER CONFIG ========== #}
	        {% set use_number      = false %}
	
	        {# STATE MODE SETTINGS #}
	        {% set state_entity    = 'fan.living_room_fan' %}
	        {% set active_value    = 'on' %}
	
	        {# OPTIONAL: NUMBER MODE SETTINGS #}
	        {% set number_entity   = 'sensor.plug_power' %}
	        {% set number_operator = '>' %}
	        {% set threshold       = 0.0 %}
	        {# ========== END USER CONFIG ====== #}
	
	        {# -------- TRIGGER DECISION LOGIC -------- #}
	        {% if use_number %}
	          {% set num = states(number_entity) | float(0) %}
	          {% if number_operator == '>' %}
	            {% set trigger_active = (num > threshold) %}
	          {% elif number_operator == '<' %}
	            {% set trigger_active = (num < threshold) %}
	          {% elif number_operator == '=' %}
	            {% set trigger_active = (num == threshold) %}
	          {% elif number_operator == '>=' %}
	            {% set trigger_active = (num >= threshold) %}
	          {% elif number_operator == '<=' %}
	            {% set trigger_active = (num <= threshold) %}
	          {% else %}
	            {% set trigger_active = false %}
	          {% endif %}
	        {% else %}
	          {% set sensor_entity = state_entity %}
	          {% if states(sensor_entity) == active_value %}
	            {% set trigger_active = true %}
	          {% else %}
	            {% set trigger_active = false %}
	          {% endif %}
	        {% endif %}
	        {# ------ END TRIGGER DECISION LOGIC ------ #}
	
	        {# Fan speed still based on THIS card's entity percentage #}
	        {% set speed = state_attr(config.entity, 'percentage') | int(0) %}
	
	        {% if trigger_active %}
	          {% if speed == 0 and is_state(config.entity, 'off') %}
	            --shape-animation: fan-ultra-idle 3s ease-in-out infinite;
	          {% else %}
	            {% set dur = 1.2 - (speed / 120) %}
	            --shape-animation: fan-ultra-spin {{ [0.25, dur] | max | round(2) }}s linear infinite;
	          {% endif %}
	          --fan-warp-animation: fan-warp 1.4s ease-in-out infinite;
	          --fan-stream-animation: fan-stream 1.6s ease-in-out infinite;
	          opacity: 1;
	        {% else %}
	          --shape-animation: none;
	          --fan-warp-animation: none;
	          --fan-stream-animation: none;
	          opacity: 0.7;
	        {% endif %}
	
	        transform-origin: 50% 50%;
	        position: relative;
	      }
	
	      .shape::before,
	      .shape::after {
	        content: '';
	        position: absolute;
	        inset: 0;
	        border-radius: inherit;
	        pointer-events: none;
	      }
	
	      .shape::before {
	        animation: var(--fan-warp-animation);
	      }
	
	      .shape::after {
	        mix-blend-mode: screen;
	        animation: var(--fan-stream-animation);
	      }
	
	      @keyframes fan-ultra-spin {
	        0% {
	          transform: rotate(0deg) scale(1);
	          filter: drop-shadow(0 0 3px rgba(var(--fan-color-rgb), 0.8));
	        }
	        20% { transform: rotate(72deg) scale(1.03); }
	        40% { transform: rotate(144deg) scale(1.05); }
	        60% { transform: rotate(216deg) scale(1.03); }
	        80% { transform: rotate(288deg) scale(1.02); }
	        100% {
	          transform: rotate(360deg) scale(1);
	          filter: drop-shadow(0 0 5px rgba(var(--fan-color-rgb), 1));
	        }
	      }
	
	      @keyframes fan-ultra-idle {
	        0%   { transform: rotate(-6deg); }
	        50%  { transform: rotate(6deg); }
	        100% { transform: rotate(-6deg); }
	      }
	
	      @keyframes fan-warp {
	        0% {
	          box-shadow:
	            0 0 0 0 rgba(var(--fan-color-rgb), 0.7),
	            0 0 0 0 rgba(var(--fan-color-rgb), 0.25);
	        }
	        40% {
	          box-shadow:
	            0 0 0 8px rgba(var(--fan-color-rgb), 0.4),
	            0 0 24px 6px rgba(var(--fan-color-rgb), 0.25);
	        }
	        100% {
	          box-shadow:
	            0 0 0 18px rgba(var(--fan-color-rgb), 0.0),
	            0 0 40px 12px rgba(var(--fan-color-rgb), 0.0);
	        }
	      }
	
	      @keyframes fan-stream {
	        0% {
	          box-shadow:
	            20px 0 16px -12px rgba(var(--fan-color-rgb), 0),
	           -20px 0 16px -12px rgba(var(--fan-color-rgb), 0);
	        }
	        50% {
	          box-shadow:
	            24px 0 20px -10px rgba(var(--fan-color-rgb), 0.6),
	           -24px 0 20px -10px rgba(var(--fan-color-rgb), 0.4);
	        }
	        100% {
	          box-shadow:
	            20px 0 16px -12px rgba(var(--fan-color-rgb), 0),
	           -20px 0 16px -12px rgba(var(--fan-color-rgb), 0);
	        }
	      }
	    .: |
	      mushroom-shape-icon {
	        --icon-size: 65px;
	        display: flex;
	        margin: -18px 0 10px -20px !important;
	        padding-right: 10px;
	      }
	      ha-card {
	        clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
	      }

  ```
</details>

<details>
  <summary><strong>5 - Lock</summary>

  ```yaml
type: custom:mushroom-entity-card
entity: switch.plug_6_local
tap_action:
  action: toggle
icon: mdi:lock
icon_color: red
name: Lock
card_mod:
	  style:
	    mushroom-shape-icon$: |
	      .shape {
	        {# ========== USER CONFIG ========== #}
	        {# true = number mode, false = state mode #}
	        {% set use_number      = false %}
	
	        {# STATE MODE SETTINGS #}
	        {% set state_entity    = 'switch.plug_6_local' %}
	        {% set active_value    = 'on' %}
	
	        {# OPTIONAL: NUMBER MODE SETTINGS #}
	        {% set number_entity   = 'sensor.plug_power' %}
	        
	        {# '>' '<' '=' '>=' '<=' #}
	        {% set number_operator = '>' %}
	        
	        {% set threshold       = 0.0 %}
	        {# ========== END USER CONFIG ====== #}
	
	        {# ---------- TRIGGER DECISION LOGIC ---------- #}
	        {% if use_number %}
	          {% set num = states(number_entity) | float(0) %}
	          {% if number_operator == '>' %}
	            {% set trigger_active = (num > threshold) %}
	          {% elif number_operator == '<' %}
	            {% set trigger_active = (num < threshold) %}
	          {% elif number_operator == '=' %}
	            {% set trigger_active = (num == threshold) %}
	          {% elif number_operator == '>=' %}
	            {% set trigger_active = (num >= threshold) %}
	          {% elif number_operator == '<=' %}
	            {% set trigger_active = (num <= threshold) %}
	          {% else %}
	            {% set trigger_active = false %}
	          {% endif %}
	        {% else %}
	          {% set trigger_active = (states(state_entity) == active_value) %}
	        {% endif %}
	        {# ---------- END TRIGGER LOGIC ---------- #}
	
	
	        {% if trigger_active %}
	          {% if is_state(config.entity, 'locked') %}
	            --shape-animation: lock-secure 2.3s ease-in-out infinite;
	          {% elif is_state(config.entity, 'unlocking') or is_state(config.entity, 'locking') %}
	            --shape-animation: lock-action 0.7s ease-in-out infinite;
	          {% else %}
	            --shape-animation: lock-open 1.6s ease-in-out infinite;
	          {% endif %}
	
	          opacity: 1;
	
	          /* HUGE glow system */
	          --glow-1: 0 0 25px 8px rgba(var(--rgb-{{ config.icon_color }}), 1);
	          --glow-2: 0 0 55px 18px rgba(var(--rgb-{{ config.icon_color }}), 0.7);
	          --glow-3: 0 0 95px 35px rgba(var(--rgb-{{ config.icon_color }}), 0.45);
	          --glow-anim: lock-ultraglow 2.2s ease-in-out infinite;
	
	        {% else %}
	          --shape-animation: none;
	          --glow-1: none;
	          --glow-2: none;
	          --glow-3: none;
	          --glow-anim: none;
	          opacity: 0.6;
	        {% endif %}
	
	        transform-origin: 50% 50%;
	        position: relative;
	      }
	
	      /* Glow layers */
	      .shape::before,
	      .shape::after {
	        content: '';
	        position: absolute;
	        inset: -8px;
	        border-radius: inherit;
	        pointer-events: none;
	      }
	
	      .shape::before {
	        box-shadow: var(--glow-1), var(--glow-2), var(--glow-3);
	        animation: var(--glow-anim);
	      }
	
	      .shape::after {
	        inset: -18px;
	        box-shadow: 0 0 120px 40px rgba(var(--rgb-{{ config.icon_color }}), 0.25);
	        opacity: 0.9;
	        animation: var(--glow-anim);
	      }
	
	      @keyframes lock-ultraglow {
	        0% {
	          opacity: 0.9;
	          filter: brightness(1);
	        }
	        50% {
	          opacity: 1;
	          filter: brightness(1.4);
	        }
	        100% {
	          opacity: 0.9;
	          filter: brightness(1);
	        }
	      }
	
	      @keyframes lock-secure {
	        0%   { transform: rotate(0deg) scale(1); }
	        15%  { transform: rotate(-12deg) scale(1.02); }
	        30%  { transform: rotate(2deg) scale(1.03); }
	        40%  { transform: rotate(0deg) scale(1); }
	        100% { transform: rotate(0deg) scale(1); }
	      }
	
	      @keyframes lock-action {
	        0%   { transform: rotate(-25deg) scale(0.96); }
	        50%  { transform: rotate(25deg) scale(1.04); }
	        100% { transform: rotate(-25deg) scale(0.96); }
	      }
	
	      @keyframes lock-open {
	        0%   { transform: rotate(10deg); }
	        50%  { transform: rotate(18deg); }
	        100% { transform: rotate(10deg); }
	      }
	    .: |
	      mushroom-shape-icon {
	        --icon-size: 65px;
	        display: flex;
	        margin: -18px 0 10px -20px !important;
	        padding-right: 20px;
	      }
	      ha-card {
	        clip-path: inset(0 0 0 0 round var(--ha-card-border-radius, 12px));
	      }

  ```
</details>
