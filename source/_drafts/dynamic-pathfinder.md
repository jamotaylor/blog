---
title: Simple Seeker 0.1.0
---
I was working on a game - way back - and I needed to have the enemies do some simple pathfinding.

The problem with the pathfinding options out there, or at least the ones I know about, is that they require some kind of mesh to be created before pathfinding can begin.

Take the standard Unity Nav Mesh [link] for example, which requires "baking" the entire level in the Unity Editor and loading the mesh with the scene at runtime.  This only works for static items - as in items that will not be able to move during gameplay.

Or, take the A* Pathfinding project [link] for example - we used this Unity package extensively in Vicious Attack Llama Apocalypse [link]. The package comes with various types of mesh creation options (which it refers to as grids), and does allow some degree of dynamic mesh creation, but the game level needs to be continually re-scanned during gameplay to update the mesh.

What I essentially wanted was something very simple that I could literally just dump in a scene and it would just work, with both static and dynamic objects, without the need to do any scanning.

I started making a submarine (it was an aircraft-carrier game) that implemented this basic concept, and it actually kind of worked. Fast-forward many months later and you have *Simple Seeker*.

{% raw %}

<iframe src="SimpleSeeker_v0.1.0_Demo/index.html" 
style="border:0px #000000 none; width:100%; height:600px; max-height:66vh;" 
name="Simple Seeker" scrolling="no" frameborder="1">
</iframe>

{% endraw %}