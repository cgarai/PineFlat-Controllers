# PineFlat-Controllers

This repository contains the Click PLD (Programmable Logic Device) controller software used in two systems on our Pine Flat house.

SolarDHWRadiant.ckp  controls the house radiant heat system and hot water recirculation.  
  Radiant: There are 5 radiant zones.  Currently the two downstairs zones and the 3 upstairs zones are ganged onto 2 Nest thermostats.
    Water is circulated on thermostat inputs.
    Water temperature is set as a function of outdoor temperature (known as "outdoor reset" in radiant HVAC) using a thermistor.
    Water temperature is controlled by a variable speed circulator that circulates water from the hot water tank to a heat exchanger
    that isolates the domestic hot water from the radiant system water.  The temperature measurement point is at the output of the heat 
    exchanger on the radiant side using a thermistor.   
  Hot water recirculation takes a simple pushbutton input and turns on a circulator that circulates water for a fixed time (~2min).
  
  Well_and_Pressure_Pump_Controls.ckp controls household water pressure and keeping the storage tank full
    Storage tank: A float switch is used as an input to signal the system to turn on the well pump and refill the tank.
    
    Household Pressure:  The fire department's requirement that the house have 36gpm at 55psi created difficult situation for a simple 
    pressure switch and pressure tank in a typical system.  Since the pump and tank are located some distance from the house, 36gpm 
    causes about 15-20 psi drop in the 2" line from the pump to the house.  In order to not have to set the household pressure at 70-75 psi
    a system is implemented that senses the pump output to sense the demand.  
    There are 3 stages:
      1.  For low or zero water demand, the system acts like a pressure switch based system, turning on at a low pressure (~35psi) and going 
          into closed loop mode at a high pressure (55psi) setpoint.  It stays here for a time period and if the power used by the pump is low
          then it shuts off.
      2.  Before the system shuts off at the high pressure, it stays in a closed loop mode to keep the pressure at the high point.  If it senses
          moderate amount of power being used it stays on in closed loop mode until the power drops off.  If the power stays below a setpoint 
          for a time period, it turns the pump off and goes back into Stage 1.
      3.  If while in open loop mode it senses pump power over a threshold for a period, it sets the closed loop pressure to 75psi which is 
          to the maximum of the pump output.  In this mode the pump can deliver 36gpm at 55psi.  The system exits this mode when the power
          used by the pump drops down below a setpoint indicating a reduction of use and either goes into closed loop at the 55psi or shuts off
          
      
