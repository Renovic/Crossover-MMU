# Crossover MMU
Multi-Material Unit with outputs to multiple printers

## Design Constraints and Initial Idea (5/30)
My goal is to create an MMU that has outputs for multiple printers. The main benefit over a traditional mmu is that it allows you to automatically switch filament between printers and function like a regular mmu for each printer. My usecase will mainly be the former, but I might use the mmu functionality for simple/small multicolor prints.

The main functionality I want is:
- A row of input filaments
- A row of outputs that lead to printers
- Ability to swap an output to any available filament 

<img width="624" height="506" alt="image" src="https://github.com/user-attachments/assets/8b91db76-c2aa-4f99-a312-bbfc95a4796c" />

My first idea for this was a system that can pick up small output modules and move them over each other to cross over othe modules to access all avialable filaments. However, I quickly realized the issue. Moving a module over another adds a twist to the ptfe tubes, and these can add up over time.

<img width="471" height="443" alt="image" src="https://github.com/user-attachments/assets/9e28e09f-ccd5-493e-a478-2157c8ffb7fd" />

Now if that left blue output module moved over the orange module, it would add another twist and this cycle could repeat indefinetly if the printers request it.

<img width="440" height="439" alt="image" src="https://github.com/user-attachments/assets/8cb1260b-7dd9-4680-b631-f43fce219f87" />

The only way to prevent this build up of twists in the ptfe tubes is by undoing twists in future moves, which can be done by moving the blue module under the orange.

<img width="462" height="452" alt="image" src="https://github.com/user-attachments/assets/1a8ed78a-23d5-44ff-802d-b607aff1c89d" />

Another realization I had was that I could guarentee the orange and blue module always have 1 or less twists by always moving the blue module under and the orange over during any crossovers. This can scale to any number of outputs, by having a strict ordering of which modules cross over and which ones cross under.

This allows me to essentially give each module its own track and not worry about keeping track of the twists. With 3 modules I could say A always goes over B (B goes under A), and B goes over C, which will ensure twists cancel out over time.

<img width="462" height="210" alt="image" src="https://github.com/user-attachments/assets/74a59b33-6e2b-4f4a-9e9e-54774f4a5fd6" />

Now that I have the twist issue solved I needed to figure out how these modules actually go over and under other modules.
I'd need a secondary manipulator at the bottom inorder to pickup the moving module and move it under the stationary ones, and also have some sort of handoff between the two.

<img width="450" height="285" alt="image" src="https://github.com/user-attachments/assets/40b405f4-2ea1-4134-877e-12d4d79bcf54" />

There's also a couple of other parts to an MMU other than just the selector:
- Filament drive gear
- Moving the drive gear
- Filament Sensor

For the extruder part, im taking inspiration from the ANNEX Tradrack:
<img width="635" height="588" alt="image" src="https://github.com/user-attachments/assets/16b8461d-c499-444e-a903-e0e8ef00d3bb" />
It uses a bmg gearset in the moving carriage, and a bearing as a smooth idler in each filament lane.
Theres a couple of other designs, like the ERCF which uses a main drive axle and press the idler into the filament using the selector, but I think the tradrack solution is the easiest to scale and cheapest per module.

As for moving the drive gear, I'll be moving it vertically linearly, instead of using a servo like the tradrack. This will also serve the purpose of moving the modules up and down.

As for the filament sensor I'm taking inspiration from Tradrack again. Using a small 4mm ball and a switch is simple and reliable. However, I can't put switches in each output module as it would significantly increase the size. So I decided on having the filament sensor attach at the same time as the filament drive gears contact the filament, allowing me to use one switch.

<img width="448" height="395" alt="image" src="https://github.com/user-attachments/assets/b85a6952-6a96-4554-a7cc-5de652684d2b" />

**Hours Spent: 2**

**Total: 2** 

## Output Module & Filament Lane
I started with the output module which is pretty simple. Has a ecas coupler for the ptfe tube, and 5 magnets. 2 on top and bottom for the manipulators to move the output module and the last at the rear to attach to the filament lane.
<img width="594" height="517" alt="image" src="https://github.com/user-attachments/assets/6fe0a56a-f604-42de-9a6b-eb9d43698d46" />
<img width="498" height="337" alt="image" src="https://github.com/user-attachments/assets/4a630e99-73e1-4d3a-b325-e68fccc5cf4b" />

The only thing to add to the output module is the nfc tag so it can automatically tell which output is in which filament lane.

Next I worked on the filament lanes.

<img width="660" height="515" alt="image" src="https://github.com/user-attachments/assets/5203184b-451b-4ab4-83d2-d7e3a5a5b155" />

They have an opening for the bmg drive gears to push the filament and a bearing as a smooth idler.

<img width="578" height="448" alt="image" src="https://github.com/user-attachments/assets/86901196-1d12-4722-acb5-a47c72bd4253" />

I also had an opening for the filament sensor to go around the filament.

At the front there's the dock for the output module and a magnet to hold the module in its intermidiate step when switching between the upper manipulator and the lower manipulator.
<img width="476" height="310" alt="image" src="https://github.com/user-attachments/assets/bafd86be-9d35-48a6-b7c6-1a85831a1162" />

But then I realized I needed space for the lower manipulator to slide off of the output module, so I added a gap here. I also added numbers to the filament lanes, and a retaining wall to further prevent modules from falling/moving when filament goes through them.

<img width="702" height="543" alt="image" src="https://github.com/user-attachments/assets/14974046-4128-4127-b795-cd612bd1ee80" />

**Hours Spent: 5**

**Total: 7**

