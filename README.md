# Crossover-MMU
Multi-Material Unit with outputs to multiple printers
<img width="1496" height="842" alt="Crossover_MMU_2026-Jun-30_11-19-06PM-000_CustomizedView32072930422_png_alpha" src="https://github.com/user-attachments/assets/e7ba376e-db49-46f3-8541-cc9f2f398cec" />

## Files
- [CAD (F3Z)](CAD/CrossoverMMU.f3z)
- [CAD (STEP)](CAD/CrossoverMMU.step)
- [BOM](<BOM.csv>)

<img width="1239" height="697" alt="Crossover_MMU_2026-Jun-30_11-09-26PM-000_CustomizedView48409742691_png_alpha" src="https://github.com/user-attachments/assets/7d1164d2-8bc2-4b79-81d6-a40e0a724fef" />

## Why I made this
I don't do a lot of multicolor printing, but I often change filaments and do simple 2-color prints. So I wanted an MMU to automate those. That's when I came up with the idea of having outputs to multiple printers. 

It was also interesting to solve the problem of tangling PTFE tubes (more about this in my [JOURNAL.md](/JOURNAL.md) ).

## Parts of the MMU
There are 3 axis: 
- X axis: Lane selector
- Z axis: For engaging the drive gear and moving the output modules
- Extruder: BMG reduction

There are (Rail Length-50mm)/25 filament lanes, and an adjustable amount of outputs. Each of them is labeled to keep track of the printers they go to.

It will require buffers or rewinders on each filament lane. I'm using filamentalist rewinders since the ptfe tube length can vary between printers. Buffers only have a set range so they are less flexible than rewinders.

## Assembly:
Most things are pretty straightforward to assemble based on the CAD, but here are a couple of things to note:
- Assemble the filament lanes on the extrusion before attaching the extrusion to the Z-joints
- Mount the extruder carriage to the rail before attaching the blue extruder housing

## References/Inspiration:
- Tradrack MMU: https://github.com/Annex-Engineering/TradRack/tree/main
- Filamentalist Rewinders: https://github.com/Carrot-collective/ERCF_v2/tree/master/Recommended_Options/Filamentalist_Rewinder
