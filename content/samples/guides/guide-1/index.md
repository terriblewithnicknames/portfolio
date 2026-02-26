---
title: Modelling a printable wheel for a scale car
date: 2026-01-25
bread: false
moredocs: false
toc: true
totop: true
---

# Introduction
Aftermarket wheels are one of the most common ways to make your scale car model stand out. While there are dozens of manufacturers of aftermarket parts and hundreds of detail-up kits, it is still easy to stumble upon cool real-world wheels that nobody sells in scale.

Using a popular [Work Meister L1 3P 19 inch wheel](https://www.work-wheels.co.jp/en/search/detail/98/) as an example, I want this article to help scale car enthusiasts learn how to model wheels, moving from a reference picture to an STL file.

{{< figure
	src="tut_export_2.png"
	caption="*Final model (Basic, merged) - Front view*"
	alt="A picture of the basic version of the model imported as an STL file into a slicer software" 
	width="100%"
	class="indoc-img"
	>}}

Please note that this tutorial **does not** cover:
- Polygonal modelling techniques.
- Advanced CAD modelling techniques.
- Topology optimization for rendering, animation, or games. The result is a waterproof STL file intended to become a physical object.
- 3D printing process. For guidance on printing, refer to the user manual supplied with your printer, or use a 3rd-party 3D printing service.

# Prerequisites
To successfully follow this guide, you will need:

1. Basic understanding of geometry and CAD modelling principles.

2. A CAD modelling tool. Preferably, [Plasticity](https://www.plasticity.xyz/), but the same techniques work in other CAD tools, like Fusion360 or SolidWorks.

3. A slicer tool of your choice to ensure the exported model is ready for print. I will be using [UltiMaker Cura](https://ultimaker.com/software/ultimaker-cura/).

## Tools and commands
Instructions provided in this doc refer to the following Plasticity commands/operations and tools. Feel free to check the official Plasticity documentation for additional guidance:

Tools:
- [Bridge curve](https://doc.plasticity.xyz/sketch/bridge-curve) 
- [Center circle](https://doc.plasticity.xyz/tool/center-circle.en)
- [Knife mode](https://doc.plasticity.xyz/tool/sketching-essentials.en#use-knife-mode)
- [Line](https://doc.plasticity.xyz/tool/line)
- [Regular polygon](https://doc.plasticity.xyz/tool/polygon.en)

Commands:
- [Boolean](https://doc.plasticity.xyz/solid/boolean.en)
- [Delete redundant topology](https://doc.plasticity.xyz/solid/delete-redundant-topology#delete-redundant-topology-solid)
- [Duplicate](https://doc.plasticity.xyz/common/duplicate.en)
- [Extrude](https://doc.plasticity.xyz/solid/extrude.en)
- [Fillet](https://doc.plasticity.xyz/solid/fillet-shell)
- [Group](https://doc.plasticity.xyz/plasticity-essentials/outliner.en#group-objects)
- [Imprint](https://doc.plasticity.xyz/solid/imprint-curve-body)
- [Join](https://doc.plasticity.xyz/sketch/join) 
- [Loft](https://doc.plasticity.xyz/solid/loft.en)
- [Match face](https://doc.plasticity.xyz/solid/match-face.en)
- [Mirror](https://doc.plasticity.xyz/common/mirror.en) 
- [Move](https://doc.plasticity.xyz/common/move.en)
- [Offset](https://doc.plasticity.xyz/solid/offset-edge.en) 
- [Pipe](https://doc.plasticity.xyz/solid/pipe.en) 
- [Push](https://doc.plasticity.xyz/solid/offset-face.en#basic-usage)
- [Radial array](https://doc.plasticity.xyz/common/radial-array)
- [Rename](https://doc.plasticity.xyz/plasticity-essentials/outliner.en#rename-objects) 
- [Revolve](https://doc.plasticity.xyz/solid/revolve.en) 
- [Scale](https://doc.plasticity.xyz/common/scale) 
- [Switch view](https://doc.plasticity.xyz/plasticity-essentials/operating-the-3d-viewport) 
- [Trim](https://doc.plasticity.xyz/sketch/trim.en) 
- [Unjoin](https://doc.plasticity.xyz/solid/unjoin-faces) 

# Preparation
Before we get to modelling, let me share a few conclusions that my scale modelling journey led me to.

## Decide on the scale
Decide on the scale of the model before actually modelling it. It is always easier to build correct shapes from scratch than refit the existing geometry. 

In this tutorial, we are going with a **1/24** scale.

## Set goals
Aside from a few real-world constraints, there are little to no rules to hobby modelling. To follow a reference meticulously, deviate from it here and there, or not use it at all - is up to you. However, like in any other craft, it is instrumental to ensure that every decision you make gets you closer to the goals set for the model.

The goals we set for our model are:
- Be scale-accurate to fit any car model in the same scale.
- Resemble the original product as closely as possible.
- Respect 3D printing constraints to ensure safe printing.

Where possible, I will supply the step-by-step instructions with precise dimensions and tool/command settings. You can follow them if you want to reach these goals with me.

> [!Note] If a parameter/setting is not specified for a tool, operation, or command, leave it as is.

## Balance details
The right combination of printer and printing material can offer fascinating precision, helping achieve high resemblance to the original part. However, going above a certain level of detail is **rarely justified** in the most common car modelling scales (1/24, 1/18, 1/35, etc.). Depending on how a model (car body, wheel, engine kit, seat, and so on) is produced (diecast, resin molding, 3D printing, etc.), and how small its scale is, manufacturers can simplify the geometry and distort the model's proportions where the hardware requires or the customers accept.

To increase your chances of a successful print: 
- Avoid modelling geometry less than **0.2mm** in depth or wall thickness.
  Such surfaces and shapes tend to come out warped, rendering the model unusable.
- Don't detail geometry that won't be visible or useful.
  Traditionally, a car wheel is fixed on a wheel hub and wrapped in a tire. No one will see the inside of that wheel.
- When analyzing references, try to break the object into simple shapes. Model them first, then add details gradually.
- Slightly enhance the model features as the scale goes smaller.
  The smaller the object, the less apparent its curvature appears to the human eye.

Be mentally prepared for your model not coming out perfect on the first try. Oftentimes, the sweet spot for the model's detail, fitment, and proportions is found through test prints. Test prints, printing defects, and other occasional hiccups are an **essential** part of 3D printing.

A better understanding of real-world manufacturing practices will also help your models. The wheel chosen for this tutorial consists of 3 parts (hence "3PIECE" in the name). However, since scale allows for geometry simplification, we can disregard the non-functional parts of the wheel design and safely split the model into two parts: a **disc** and a **barrel**.

{{< figure
	src="tut_wheel_types.png"
	link="https://apexwheels.com/blog/technical-discussion/3-piece-2-piece-and-1-piece-wheels-explained"
	caption="*Common car wheel types (from left to right): 3-piece, 2-piece, Monoblock*"
	alt="A picture of the common car wheel types" 
	width="100%"
	class="indoc-img"
>}}

## Find references
To model an existing wheel, you will need a good reference picture of it in the right size and spec (the same wheel can come in different variants). Ideally, you want the wheel to be shot directly from the front. The less twisted the perspective, the better. Thankfully, the [official page](https://www.work-wheels.co.jp/en/search/detail/98/) contains a lot of high-quality photos of the Meister L1 wheel we are about to model. Let's see what makes a good primary reference image.

{{< figure
	src="tut_ref_1.png"
	link="https://www.work-wheels.co.jp/img_save/main/58a513257fbfd.jpg"
	caption="*Reference example 1*"
	alt="An angled photo of the Work Meister L1 wheel on a car" 
	width="100%"
	class="indoc-img"
>}}

Here, a wheel is clearly seen, and the image is high quality. Yet the angle of the shot makes it hard to determine the wheel size and accurately trace the geometry of the spoke pattern.

At the same time, though, the picture shows the rim profile, revealing details that frontal shots hide. Is the disc face flat, concave, or convex? Where, how much. How deep the disc sits in the barrel. On top of that, live pictures like this can help you keep the model's overall resemblance to reality in check.

With the above in mind, this is not the best candidate for a primary reference.

{{< figure
	src="tut_ref_2.jpg"
	link="https://www.work-wheels.co.jp/img_save/main/5805bdcbd7548.jpg"
	caption="*Reference example 2*"
	alt="A frontal photo of the Work Meister L1 wheel" 
	width="100%"
	class="indoc-img"
>}}

In this example, however, you can clearly see that the wheel is shot from the front and centered almost perfectly. The size is right (according to the image description), nothing obstructs the geometry, and every front-facing detail is visible. Shots like this make it a lot easier to recreate the disc face accurately.

This picture will be our primary reference. Click on the image to download it from the source.

## Set dimensions/units
As we aim for precision, our 3D tool has to be set to millimeters (mm). To set Plasticity to mm:

1. Open Plasticity and start a new file.
2. Navigate to the right sidebar (1).
3. Expand the "Units" section (2) and set the fields:
	- "Units" - "Millimeter".
	- "Grid size" - **20** or higher.
	- "Line every" - **1**.
4. Expand the "Grid" section (3) and make sure the value is "**1 mm**".

	{{< figure
		src="tut_prep_1.png"
		alt="A picture of the right sidebar in Plasticity" 
		width="50%"
		class="indoc-img"
	>}}

>[!Info] It is also helpful to have [object snapping](https://doc.plasticity.xyz/plasticity-essentials/snap.en#object-snap) enabled.

## Import the reference image
The easiest way to stay close to the original geometry is to rely on reference images. Let's import the primary reference image we previously selected and position it in the 3D viewport:

1. In Plasticity, switch the view to "Top".
2. Click the "[P-Menu](https://doc.plasticity.xyz/plasticity-essentials/file-menu)" in the upper-left corner (1), then **Import/Append** (2). Alternatively, press `Ctrl+Shift+O`.
   
	{{< figure
		src="tut_prep_2.png"
		alt="A picture of the P-Menu in Plasticity" 
		width="50%"
		class="indoc-img"
	>}}

3. Select the image file and confirm the import. The image should appear positioned according to your current 3D viewport view.
4. Create a center circle to represent the outer edge of the wheel:
	- Center point - world origin.
	- Diameter - **21.6 mm**.
	- Vertex - on the Y axis.

5. Move and scale the image so that the center of the wheel matches the world origin and the outer edge of the wheel matches the created circle.
6. To keep the project organized, rename the created circle to "Barrel outer edge".
7. Optionally:
	- To give yourself some extra space in the 3D viewport, move the image up/down along the Z axis.
	- To prevent accidental edits, lock the image by clicking the lock icon next to it in the left sidebar.
	- To have more comfortable image opacity, select the image, press `M` hotkey, tweak the "Opacity" value in the "Material" menu, and click **OK** to confirm.

The result should look like this:
{{< figure
	src="tut_prep_4.png"
	alt="A picture of the reference image imported into Plasticity" 
	width="100%"
	class="indoc-img"
>}}

# Modelling the disc
Using the reference images, we can point out that the disc is mounted on the barrel from the back and has:
- A concave front face with a slightly angled side wall.
- A conical, rounded recess in the center of the front face, with holes inside: one for a center cap (aka center bore), 5 for the lug nuts.
- Six flat, identical spokes. 
- Spoke cutouts, horizontally shaped like triangles with beveled edges; vertically, they get smaller toward the back of the disc.
- "JWL" and "VIA" manufacturing stamps on two of the spokes.
- "WORK Wheels" brand sticker on one of the spokes.

>[!Note] With rare exception, stickers are best recreated with waterslide decals, not geometry.

## Sketching disc profile
In this section, we will sketch the disc revolved profile. It represents the disc center hub, front face, spokes, side wall, and the mating face (a part where the disc and barrel touch).

{{< figure
	src="tut_disc_p_final.png"
	caption="*Complete revolved profile of the disc*"
	alt="A right-view picture of the complete revolved profile of the disc in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

### Front surface
Let's start with the front-facing line of the profile:

1. Switch the view to "Top".
2. Draw a straight line on the Y axis, placing the control points at the following spots in the reference image:
	- World origin.
	- Upper edge of the center bore.
	- Lower edge of the center bore.
	- Lower edge of the disc center recess.
	- Upper edge of the disc center recess.
	- Slightly above the upper edge of the disc side wall.
	- Above the outer edge of the mounting bolts ring.
	  
	The line should look like this:
	{{< figure
		src="tut_disc_p_1.png"
		alt="A top-view picture of the base front polyline in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

	> [!Info] **Scale affects geometry - Example 1**
	> Not all control points match the wheel curves in the reference. Some proportions have to be deliberately distorted to ensure the model looks good **and** prints without issues.

3. Switch the view to "Right".
4. Move the control points of the created polyline along the Z axis:
	{{< figure
		src="tut_disc_p_2.png"
		alt="A right-view picture of the base front polyline in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}
	- Points 1,2 - **1.4 mm** down.
	- Points 3,4 - **2 mm** down.
	- Point 5 - **0.8 mm** down.

5. Create a duplicate line between points 6 and 7, then move it **2.5 mm** down on the Z axis.
6. Connect point 6 with its duplicate (6(1)) with another straight line.
7. Trim the original 6-7 line.
8. Select all the line segments that are left and join them.

	The result should look like this:
	{{< figure
	src="tut_disc_p_3.png"
	alt="A right-view picture of the complete front polyline in Plasticity" 
	width="100%"
	class="indoc-img"
	>}}

	>[!Info] **Scale affects geometry - Example 2**
	>Angle of the side wall is too shallow to be noticed in the selected scale. Making the side wall flat will help us maintain safe wall thickness without drifting too far away from the reference.

9. Rename the line to "Front surface" and press `H` to hide it.

### Rear surface
Now, to the rear line of the profile - the back of the disc:

1. While in the "Right" view, draw a set of straight lines, starting at the world origin and moving along the Y axis. Every next line **must** begin at the previous line's last control point:
	- Line 1-2 - **3.3 mm**.
	- Line 2(1)-3 - **0.4 mm**.
	- Line 3(1)-4 - **3.6 mm**.
	- Line 4(1)-5 - **0.35 mm**.
	- Line 5(1)-6 - **1.85 mm**.

	{{< figure
		src="tut_disc_p_4.png"
		alt="A picture of the base rear polyline in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

2. Move the following lines and points on the Z axis:
	- Line 1-2 - **3.8 mm** down.
	- Line 3(1)-4 - **1.2 mm** down.
	- Point 3(1) - **0.8 mm** down.
	- Line 5(1)-6 - **3.7 mm** down.

3. Connect the control points:
	- 2 and 3(1) - with a straight line.
	- 4 and 5(1) - start at control point 4 and draw a **2 mm** vertical line down, then draw another line to 5(1). 

4. Delete lines 2(1)-3 and 4(1)-5.
5. Join the segments that are left and rename the line to "Rear surface".

	The result should look like this:
	{{< figure
		src="tut_disc_p_5.png"
		alt="A right-view picture of the complete rear polyline in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

### Joining curves
Now, let's join the halves into a revolved profile:

1. Unhide the "Front surface" line.
2. Connect both ends of the polylines with straight lines to form a closed contour of the revolved profile:
	{{< figure
		src="tut_disc_p_6.png"
		alt="A right-view picture of the connected halves of the disc profile in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

3. Group all lines, name the group "Disc profile", and hide it.

## Sketching cutout
From the reference images, we know that the spoke cutouts get smaller at the back of the disc. To replicate that, we will split the cutout into two contours/edges: front and rear. 

### Front edge
Let's start with the front edge of the cutout:

1. Switch the view to "Top".
2. Pick a cutout in the reference image. Preferably, the one that an axis splits in half. On either of the spokes surrounding the cutout, find a straight edge. Mark it with a line:
	{{< figure
		src="tut_disc_c_1.png"
		alt="A picture of the base spoke cutout line in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

3. Mirror the line on the X axis.
4. Bridge two curves at vertices close to the center. Tweak the bridging curve so it matches the cutout edge in the reference:
	{{< figure
		src="tut_disc_c_2.png"
		alt="A picture of the mirrored base spoke cutout lines in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

	>[!Info] It's okay if the mirrored lines don't land on the opposite edges in the image exactly. In this sketch, we are chasing symmetry first and foremost.

5. Create a center circle:
	- Center point - world origin.
	- Diameter - just below the edge of the disc side wall, or **15.4 mm**. 
	- Vertex - on the Y axis.

6. Bridge one of the straight lines with the created circle. Tweak the bridging curve so it stays close to the cutout edge in the reference:
	{{< figure
		src="tut_disc_c_3.png"
		alt="A picture of the bridged base spoke cutout lines in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

	>[!Info] **Scale affects geometry - Example 3**
	>Cutouts sit close to the front edge of the side wall, forming a noticeable feature of the wheel design. If recreated 1:1, the side wall might turn out too thin for printing. One way to address that without losing the resemblance is to thicken the side wall and proportionally enlarge the cutout at the front.

7. Select the top two curves and mirror them on the X axis. Coinciding control points should form a closed contour.
8. Join all curves into one and move it **2mm** up on the Z axis.
9. Rename the curve to "Front edge".

The result should look like this:
{{< figure
	src="tut_disc_c_4.png"
	alt="A picture of the complete front spoke cutout curve in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

### Rear edge
Though we are not dealing with polygons directly, it is still better to have a cleaner mesh than not. That is why we will create the rear edge of the cutout from the front one:

1. Duplicate the "Front edge" curve, rename it to "Rear edge", and hide the original.
2. Scale the "Rear edge" so that it follows the inner contour of the cutout in the reference image:
	{{< figure
		src="tut_disc_c_5.png"
		alt="A picture of the complete rear spoke cutout curve in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

	>[!Note] It may take a few scale operations to get the shape right. Try matching the curves in stages, moving from the bottom to the top.

3. Move the curve **3mm** down along the Z axis.
4. With both cutout edges complete, group them, name the group "Spoke cutout", and hide it.

## Working with solid bodies
Let's transform sketches into the disc:

{{< figure
	src="tut_disc_s_final_1.png"
	caption="*Complete disc (Basic version)*"
	alt="A picture of the complete basic version of the disc in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

### Revolving the profile
1. Unhide the "Disc profile" group.

2. Revolve the region that the profile outlines around the Z axis to form a solid.

3. Rename the solid to "Disc" and delete redundant topology from it.

4. Hide the "Disc profile" group.

The result should look like this:
{{< figure
	src="tut_disc_s_1.png"
	alt="A picture of the base disc solid in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

### Creating spokes
Next, let's use the spoke cutout sketches to reveal spokes:

1. Unhide "Spoke cutout" group.
2. Use the "Radial array" command on both cutout curves:
	- Center point - world origin.
	- Number of instances - **6**.

3. Imprint all cutout curves (original and duplicates) onto the "Disc" solid.
4. Delete duplicate cutout curves and hide the "Spoke cutout" group.
5. Select all faces that the imprinted cutout curves created on both sides of the "Disc" solid, unjoin, and delete them. "Disc" solid became a sheet.
6. Pick a cutout and loft its front and back edges with a **G0** surface. Repeat for all cutouts.
7. Select all sheets with `A` and join them into a solid. "Disc" is now solid again.
8. With the basic shape of the disc complete, add fillets to the following edges:

	| Edge        | Fillet Shape | Fillet Distance |
	|:----------- |:----------- |:---- |
	| Lower edge of the center bore | G2 | **0.2mm** |
	| Lower edge of the disc center recess | G2 | **0.2mm** |
	| Upper edge of the disc center recess | G2 | **0.2mm** |
	| Front edges of spoke cutouts | G2 | **0.1mm** |

	The result should look like this:
	{{< figure
		src="tut_disc_s_2.png"
		alt="A picture of the base disc solid with spoke cutouts and fillets in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

9. Add support fillets to the following edges on the back of the disc:

	| Edge                              | Fillet Shape | Fillet Distance |
	| --------------------------------- | ------------ | --------------- |
	| Outer edge of the wheel hub       | G2           | **0.3 mm**      |
	| Edge between spokes and side wall | G2           | **0.2 mm**      |

	{{< figure
		src="tut_disc_s_3.png"
		alt="A picture of rear of the base disc solid in Plasticity, target edges highlighted" 
		width="100%"
		class="indoc-img"
	>}}

### Adding lug nuts
First, create lug nut holes:
   
1. Switch the view to "Top".
2. Create a center circle to outline the bolt pattern:
	- Center point - world origin.
	- Diameter - matches the center of a lug nut hole in the reference image, or **4.55 mm**.
	- Vertex - on the Y axis.

3. Create another center circle for a lug nut hole:
	- Center point - vertex of the bolt pattern circle.
	- Diameter - **1.1 mm**.
	- Vertex - on the Y axis.

4. Hide or delete the bolt pattern circle.
5. Extrude the region that the lug nut hole circle creates **2.5 mm** down on the Z axis to get a cylinder.
6. Hide or delete the lug nut hole circle.
7. Use the "Radial array" command on the extruded cylinder:
	- Center point - world origin.
	- Number of instances - **5**.

8. Use the "Boolean" command to cut the lug nut holes in the disc:
	- Target body - "Disc" solid.
	- Tool bodies - lug nut hole cylinders.
	- Operation - "Diff".
	- Keep tools - "off".

>[!Note] If you don't follow the dimensions precisely, make sure the lug nut holes cut at least **~0.5 mm** deep into the "Disc" solid.

The result should look like this:
{{< figure
	src="tut_disc_s_4.png"
	alt="A picture of the base disc solid with spoke cutouts, fillets, and lug nut holes in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

Then, add lug nuts:
1. While in the "Top" view, pick a lug nut hole and create a regular polygon inside it:
	- Center point - the lug nut hole center.
	- Number of vertices - **6**.
	- Size - **0.95 mm**.
	- Orientation - on the Y axis.

2. Extrude the region that the polygon creates **0.5 mm** up on the Z axis.
3. Hide or delete the polygon curve.
4. Using "Knife" mode, draw a center circle:
	- Center point - center of the polygon's top face.
	- Diameter - **0.7 mm**.
	- Vertex - on the Y axis.

5. Push the face that the circle created **0.3 mm** up on the Z axis, creating a cylinder.
6. Add fillet to the upper edge of the created cylinder:
	- Shape - G2.
	- Distance - **0.15 mm**.

7. Connect the bottom face of the extruded polygon with the lug nut hole surface using the "Match face" command.
8. Use the "Radial array" command on the extruded body:
	- Center point - world origin.
	- Number of instances - **5**.

9. Use the "Boolean" command to merge the disc and the lug nuts:
	- Target body - "Disc" solid.
	- Tool bodies - lug nuts solids.
	- Operation - "Union".
	- Keep tools - "off".

>[!Note] The selected style of lug nuts is not obligatory. Should you replace them with something else, keep in mind the goals set for the model.

The result should look like this:
{{< figure
	src="tut_disc_s_5.png"
	alt="A picture of the base disc solid with spoke cutouts, fillets, and lug nuts in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

### Detailing center bore
Typically, the center bore is either empty or covered by a center cap. 

To model it empty:

1. Switch the view to "Top".

2. Offset the upper edge of the center bore. Distance - **0.3 mm**. This will create a new face in the center of the surface.

3. Push the created face **1 mm** down on the Z axis.

The result should look like this:
{{< figure
	src="tut_disc_s_final_1.png"
	alt="A picture of the complete basic version of the disc in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

At this point, this wheel part is **sufficiently detailed** for the selected scale. If you are satisfied with the result, feel free to focus on the other part - the barrel.

### Optional details
If you want to go the extra mile, let's add some optional details: the manufacturing stamps and the center cap.

>[!Note] This section covers geometry outside the safe boundaries of 3D printing in 1/24 scale. How well these details print highly depends on the printer and printing material.

To add the manufacturing stamps:

1. Hide everything but the primary reference image of the wheel.

2. Outline the "JWL" and "VIA" stamps with lines.

3. Join each contour and rename them to "JWL" and "VIA" respectively.

4. Hide the reference image and unhide the "Disc" solid.

5. Imprint the contours onto the spokes of the "Disc" solid. Hide both curves once done.

6. Push faces the projected curves created **0.08 mm** down into the "Disc" solid.

The result should look like this:
{{< figure
	src="tut_disc_s_7.png"
	alt="A close-up picture of the manufacturing stamps modeled on the disc in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

To add the center cap:

1. Undo/remove the geometry that represents the empty center bore so that its face is flat.
2. Push the center bore face **0.15 mm** down and hide the "Disc" solid.
3. With the help of commands and techniques we used previously, sketch the cap profile and revolve it around the Z axis into a solid:
	{{< figure
		src="tut_cap_p_1.png"
		alt="A picture of the complete center cap revolved profile in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

4. Group the sketch lines as "Cap profile" and hide it.
5. Rename the solid to "Cap" and delete redundant topology from it.
6. Snap the "Cap" solid to the center bore surface of the "Disc" solid.
7. While in the "Top" view, import the below primary reference image:

	{{< figure
		src="tut_cap_ref_1.png"
		link="https://www.jdmconcept.com.au/part/work/meister-center-cap"
		caption="*Center cap primary reference*"
		alt="A frontal close-up photo of the Work Meister center cap" 
		width="100%"
		class="indoc-img"
	>}}

	{{< figure
		src="tut_cap_ref_2.png"
		link="https://www.workwheelsuk.com/work-s1r-m1r-m13p-centre-cap.html"
		caption="*Center cap secondary reference*"
		alt="An angled close-up photo of the Work Meister center cap" 
		width="100%"
		class="indoc-img"
	>}}

8. Scale and center the imported image in relation to the "Cap" solid.
9. Recreate the "MEISTER" logo and the surrounding grooves with lines:
	{{< figure
		src="tut_cap_p_2.png"
		alt="A picture of the center cap contours sketched in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

	>[!Info] **Scale affects geometry - Example 4**
	>To increase the likelihood of the cap grooves being visible on the print, I slightly enhance them, while ignoring even smaller details, like the gap between the cap parts.

10. Join the lines into letters and group all the curves into a "Logo" group.
11. Imprint all curves onto the "Cap" solid. Hide the "Logo" group once done.

>[!Note] **It may take a few tries**
>If a curve breaks upon projection, either recreate it to ensure correct normal orientation, or move curves a bit more apart, so that the projected vertices are not too close to each other.

12. Push the projected letters **0.1 mm** down into the "Cap" solid.
13. Pipe the projected grooves:
	- Vertex count - **4**.
	- Section size - **0.05 mm**.

14. Use the "Boolean - Union" command to merge the "Disc" and the "Cap" solids.

With all optional details added, the disc should look like this:

{{< figure
	src="tut_disc_s_final_2.png"
	caption="*Complete disc (Advanced version)*"
	alt="A picture of the complete advanced version of the disc in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

# Modelling the barrel
From the reference pictures, we know that the barrel has:
- A step outer lip/outer rim slightly angled outwards. 
- 45 mounting bolts.
- A valve stem.

>[!Note] Valve stems are best modeled and printed separately. Making them a part of the wheel model complicates printing.

## Sketching barrel profile
In this section, we will sketch the barrel revolved profile to transform it into a solid body later. This profile represents the inner and outer surfaces of the barrel, including the outer lip and the mating face (a part where the disc and barrel touch).

{{< figure
	src="tut_barrel_p_final.png"
	caption="*Complete revolved profile of the barrel*"
	alt="A picture of the complete revolved profile of the barrel in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

### Inner surface
To create the inner surface of the barrel:

1. Hide everything but the "Barrel outer edge" circle we created earlier.
2. Switch the view to "Right".
3. Draw a set of straight lines, starting at the vertex of the "Barrel outer edge" circle and moving down on the Z axis. Every next line **must** begin at the previous line's last control point:
	- Line 1-2 - **0.2 mm**.
	- Line 2(1)-3 - **0.2 mm**.
	- Line 3(1)-4 - **1.3 mm**.
	- Line 4(1)-5 - **0.3 mm**.
	- Line 5(1)-6 - **1.4 mm**.
	- Line 6(1)-7 - **1 mm**.
	- Line 7(1)-8 - **7.6 mm**.
	  
	The line should look like this:
	{{< figure
		src="tut_barrel_p_1.png"
		alt="A picture of the base inner polyline of the barrel in Plasticity" 
		width="100%"
		class="indoc-img"
	>}}

1. Move the following lines and control points on the Y axis:
	- Line 1-2 - **0.2 mm** left.
	- Line 3(1)-4 - **0.8 mm** left.
	- Point 4 - **0.1 mm** left.
	- Line 5(1)-6 - **1.75 mm** left.
	- Point 5(1) - **0.1 mm** right.
	- Line 6(1)-7 - **2.6 mm** left.
	- Line 7(1)-8 - **1.2 mm** left.

2. Trim or delete lines 2(1)-3 and 4(1)-5.
3. Connect the remaining lines with more straight lines and join them all into one "Inner surface" polyline.

The result should look like this:
{{< figure
	src="tut_barrel_p_2.png"
	alt="A picture of the completed inner polyline of the barrel in Plasticity" 
	width="100%"
	class="indoc-img"
>}}

### Outer surface
To create the outer surface of the barrel, draw a straight line:
- Start point - vertex of the "Barrel outer edge" circle.
- Length - **12 mm** down on the Z axis.

### Joining curves
Now, let's join the lines into a revolved profile:

1. Hide the "Barrel outer edge" circle.
2. Connect both ends of the barrel surface polylines with straight lines to form a closed contour:
	{{< figure
		src="tut_barrel_p_3.png"
		alt="A right-view picture of the connected havles of the barrel profile in Plasticity"
		width="100%"
		class="indoc-img"
	>}}

3. Join all lines into a "Barrel profile" profile.
4. Move the "Barrel profile" polyline **2 mm** up on the Z axis so that the mating faces fit with **0.1 mm** clearance:
	{{< figure
		src="tut_barrel_p_4.png"
		alt="A right-view picture of the disc and barrel profiles fitting together in Plasticity"
		width="100%"
		class="indoc-img"
	>}}

## Working with solid bodies
Let's transform sketches into the barrel:

{{< figure
	src="tut_barrel_s_final.png"
	caption="*Complete barrel*"
	alt="A picture of the complete barrel in Plasticity"
	width="100%"
	class="indoc-img"
>}}

### Revolving the profile
1. Revolve the region that the barrel profile outlines around the Z axis to form a solid.
2. Rename the solid to "Barrel" and delete redundant topology from it.
3. Hide the "Barrel profile" polyline.

The result should look like this:

{{< figure
	src="tut_barrel_s_1.png"
	alt="A right-view picture of the connected havles of the barrel profile in Plasticity"
	width="100%"
	class="indoc-img"
>}}

4. With the basic shape of the barrel complete, add **G2** fillets to the following outer lip edges:
	{{< figure
		src="tut_barrel_s_2.png"
		alt="A picture of the barrel in Plasticity with the target edges higlighted"
		width="100%"
		class="indoc-img"
	>}}
	
	- Edge 1 - **0.2 mm**.
	- Edges 2, 3, 4 - **0.3 mm**.

### Adding mounting bolts
Start by creating one mounting bolt:

1. Switch the view to "Top".
2. Draw a reference line, connecting the inner and outer edges of the mounting bolts ring:
	{{< figure
		src="tut_barrel_s_3.png"
		alt="A close-up picture of the mounting bolts reference line in Plasticity"
		width="100%"
		class="indoc-img"
	>}}

3. Create a center circle to represent the base of a mounting bolt:
	- Center point - midpoint of the previously created reference line.
	- Diameter - **0.6 mm**.
	- Vertex - on the Y axis.
	  
	The circle should land on the surface of the "Barrel" solid.

4. Hide or delete the previously created reference line.
5. Extrude the region that the circle creates **0.15 mm** up on the Z axis into a cylinder and hide the base circle.
6. Select the top face of the cylinder and set the "Angle adjacent" parameter in its "Push Face" context menu to **25.00**.
7. Keep the face selected and extrude it **0.2 mm** up on the Z axis to get another cylinder.
8. Add a fillet to the top edge of the extruded body: Shape - Conic, Distance - **0.1 mm**.
9. Extrude the round face that the fillet created at the top of the cylinder **0.15 mm** down on the Z axis.

The bolt should look like this:
{{< figure
	src="tut_barrel_s_4.png"
	alt="A close-up picture of the mounting bolt model in Plasticity"
	width="100%"
	class="indoc-img"
>}}

10. Use the "Radial array" command to multiply the created bolt and spread the duplicates evenly on the barrel:
	- Center point - world origin.
	- Number of instances - **45**.

11. Use the "Boolean" command to merge the barrel and the mounting bolts:
	- Target body - "Disc" solid.
	- Tool bodies - lug nuts solids.
	- Operation - "Union".
	- Keep tools - "off".

>[!Note] The selected style of mounting bolts is not obligatory. Should you replace them with something else, keep in mind the goals set for the model.

The result should look like this:

{{< figure
	src="tut_barrel_s_final.png"
	caption="*Complete barrel*"
	alt="A picture of the complete barrel in Plasticity"
	width="100%"
	class="indoc-img"
>}}

At this point, this wheel part is **sufficiently detailed** for the selected scale. If you are satisfied with the result, feel free to focus on the other part - the disc.

# Exporting the complete model
With both the disc and the barrel complete, the wheel is ready for export. You can export the parts separately so that the model is easier to paint, or merge them into one solid so it's easier to print.

If you want to merge the parts:

1. Duplicate the "Disc" and the "Barrel" solids and hide the originals.
2. Close the gap between the duplicate solids using the "Push" or "Match face" command on their mating faces:
	{{< figure
		src="tut_export_1.png"
		alt="A disc and barrel section in Plasticity with the clearance gap between them highlighted"
		width="100%"
		class="indoc-img"
	>}}

3. Use the "Boolean - Union" command to fuse the duplicate solids into one.
4. Rename the fused solid to "Assembly".

To export the solids (merged or not):

1. Select a solid you want to export.
2. Click the "[P-Menu](https://doc.plasticity.xyz/plasticity-essentials/file-menu)" in the upper-left corner (1), then **Export** (2). Alternatively, press `Ctrl+Shift+E`.
	{{< figure
		src="tut_prep_3.png"
		alt="A picture of the P-Menu in Plasticity"
		width="50%"
		class="indoc-img"
	>}}

3. Name the destination file, set the file type to "STL", and click **Save** to confirm.
4. Configure the export settings. Here are the settings that worked for me:
	- Density - **1.00**.
	- Edge angle tolerance - **0.07**.

5. Hit **OK**.

>[!Note] It may take a few tries to get the result you want. Try different export settings and patterns (e.g., exporting through other tools, like Blender).

Now, let's import the STL file into a slicer and check the model for defects. Usually, the slicer will highlight any issues with geometry and notify you about them as soon as the model is imported.

The model we created imported without any issues or alerts. The generated mesh doesn't have visible defects, overlaps, wrinkles, or overhang surfaces that need supports. 

{{< figure
	src="tut_export_2.png"
	caption="*Final model (Basic, merged) - Front view*"
	alt="A picture of the basic version of the model imported as an STL file into a slicer software" 
	width="100%"
	class="indoc-img"
>}}

>[!Info] The model is rotated **45** degrees on the X axis to mimic the position that the wheels are commonly printed in.

On the back of the model, there are a couple of red areas - places where the model needs support to print properly. Depending on the disc complexity, even top spokes may require supports. In this case, though, the amount and placement of supported areas look acceptable.

{{< figure
	src="tut_export_3.png"
	caption="*Final model (Basic, merged) - Rear view*"
	alt="A picture of the basic version of the model imported as an STL file into a slicer software" 
	width="100%"
	class="indoc-img"
>}}

The same steps apply if you export the wheel in parts:

{{< figure
	src="tut_export_4.png"
	caption="*Final model (Advanced, split) - Front view*"
	alt="A picture of the advance version of the model imported as an STL file into a slicer software" 
	width="100%"
	class="indoc-img"
>}}

Once it is clear that the model has no issues at this stage, it is considered **finished** and ready for printing.

Renders nicely too!

{{< figure
	src="tut_export_5.png"
	alt="A picture of the complete model rendered in a 3D software" 
	width="100%"
	class="indoc-img"
>}}
