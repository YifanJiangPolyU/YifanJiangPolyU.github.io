---
title: "Swing-up and Balancing Control of an Inverted Pendulum"
categories:
  - post
tags:
  - Hobby
  - tech

layout: single
---

In this mini project, nonlinear control strategies for the swing-up control of
an inverted pendulum on a cart (i.e. the cart-pole system) is explored. The
controller is verified in a simulation. Let's take a look at how it works in the
video below. Only the pendulum is visualized, and the cart is not visible.

<iframe width="1294" height="480" src="https://www.youtube.com/embed/SJh4P6uV5ag" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Source code for this project is available at <a href="https://github.com/YifanJiangPolyU/cart-pole-nonlinear-control">https://github.com/YifanJiangPolyU/cart-pole-nonlinear-control</a>

## The System

A cart-pole system is a classical example used in control engineering. It is something that looks like Figure 1.

<figure style="width: 360px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/images/2016-04-01-Inverted-Pendulum/cart-pole.png">
  <figcaption>Fig 1. The Cart Pole System (<a
  href="https://danielpiedrahita.wordpress.com/portfolio/cart-pole-control/">source</a>)</figcaption>
</figure>

A pendulum (the pole) freely rotates on a shaft mounted on a moving cart. The
shaft is NOT actuated, and the only control action applicable is a force on the
cart.

## The Algorithm

Initially, the pendulum is at its stable equilibrium position (i.e. lowest
position). The control algorithm does 2 jobs. First, it tires to move the cart
so that the pendulum swings up to the unstable equilibrium position (i.e. the
highest position). Second, it tries to balance the pendulum at its unstable
equilibrium.

The swing-up part of the job is more interesting and more challenging. The
during swing-up, the pendulum travels through highly nonlinear regions of the
state space, rendering conventional linear system techniques unusable. Further
more, the system is under-actuated, and when the pendulum is horizontal, moving
the cart does not generate any effect on the pendulum (even for a very brief
moment).

The method implemented here was mentioned in the course materials of <a
href="https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-832-underactuated-robotics-spring-2009/">MIT
OpenCourseWare 6.832, Underactuated Robotics</a> This method transforms the
swing-up control problem into the regulation problem of an energy-like term.
Namely, the energy error term is defined as follows:

<figure style="width: 254px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/images/2016-04-01-Inverted-Pendulum/energy-error.png">
</figure>

In the equation, J is the moment of inertia of the pendulum, and g is the
gravitational acceleration. Other notations are consistent with Figure 1. Note
that when the pendulum is at upright position, the energy error term goes to
zero. A regulator is then added to drive the energy error to zero, which is
effectively the same as making the pendulum approach the upright position.

When the pendulum is close enough to the upright position, the system can
effectively be replaced by its linearization about the upright position. At this
moment, the energy error regulator is simply switched off and a linear quadratic
regulator (LQR) takes over to balance the pendulum.
