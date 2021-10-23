  21-10-21 TO 23-10-21                    VSD-IAT WORKSHOP 2021                     MANDA V V S R PAVAN KUMAR
                               Band-Gap Reference Circuit (BGR) using Sky130
OVERVIEW:
In this workshop we have learned about the basics of BGR circuit design, Analog concepts and how to use opensource
tools such as Ngspice for pre-layout and post-layout simulation by writing spice code from schematic, Magic for
layout design, DRC and PEX and Ngspice for LVS verification.
SPECIFICATION:

                                PARAMETERS                                VALUES

                                 Supply voltage                              1.8V

                                  Temperature                    -40 to 125 degrees centigrade

                               Power consumption                            <60µA
                                   Off current                              <2µA

                                  Start-up time                             <2µS

                                Temp-co of Vref                            <50ppm



INTRODUCTION:
   1. Basics:
         • Low dropout voltage regulator (LDO) requires Vref which is independent of process, temperature,
              supply voltage. Vref is provided by PVT (Process, Voltage, Temperature) independent biasing
              circuits.
                   Typical Temperature coefficient 10-50 ppm/°C (approx. 10-50µV/°C for Vref = 1V)
                                  Typical power supply voltage 40-60dB (10-1 mV/V)
         • These blocks require currents as well which is provided by PVT independent biasing circuit.
      Voltage – Divider:
         • Good temperature coefficient due to resistors.
         • But if we see it with respect to variation in Vdd it will be an undesirable power supply system.
                                                                     𝑅2
                                                  Vref = Vdd * 𝑅1+𝑅2
                                  𝑉𝑑𝑑 𝜕𝑉𝑟𝑒𝑓
                               = 𝑉𝑟𝑒𝑓* 𝜕𝑉𝑑𝑑 = 1 (undesirable power supply system)

       Forward biased PN junction:




                                             Fig1. Forward bias PN Junction
           •   Current mirror is used in order to improve good power supply rejection.
           •   Temperature coefficient approx. equivalent to 2333ppm/°C which is dominated by PNP transistor.
                                                          𝐾𝑇      𝑉𝑑𝑑
                                                  VEB =    𝑞
                                                             ∗ ln 𝑅1∗𝐼𝑠
                                                          𝑑𝑉𝑟𝑒𝑓 𝑉𝑑𝑑
                                                     =    𝑑𝑣𝑑𝑑
                                                                ∗ 𝑉𝑟𝑒𝑓
                                                           1
                                                     =      𝐼   <1
                                                          ln
                                                           𝐼𝑠
                         *Is = saturation Current *Vref = reference voltage
                          *Vdd = supply voltage

2. Introduction to Band-Gap Reference circuit (BGR):




                                    Fig2. General schematic and graph of BGR
        •   Temperature Independent Voltage reference circuit is widely used in Integrated circuits.
        •   Produces constant voltage regardless of power supply variation, temperature changes and circuit
            loading.
        •   Output voltage will be close to 1.2V (close to bandgap energy of silicon at 0K)
        •   All applications starting from Analog, Mixed signal, Digital, RF and System on Chip (SOC).




                                            Fig 3. BGR principle block diagram

                                                   Vref = VCTAT + K*VPTAT
        Why BGR?

            •    A battery is not suitable to use as a reference voltage source because voltage drops over time.
            •    A typical power supply is also not suitable because of its noisy output or residual ripple.
            •    A voltage reference IC used buried Zener diode is also not used due to discrete design requires
                 additional components and high filtering circuits due to higher thermal noise.
            •    Low voltage Zener diode is also not applicable.
         Solution:

             •   We can use a Band-gap reference circuit which is integrated in bulk CMOS, Bi-Cmos and
                 Bipolar technologies without the use of external components.
              Applications:

                 •   Low Dropout voltage regulator (LDO)
                 •   DC -DC buck converter
                 •   Analog to Digital Converter (ADC)
                 •   Digital to Analog Converter (DAC)
              Types of BGR:
                     Architecture wise BGR can be designed in 2 ways.
                 ❖ Self- biased current mirror
                 ❖ By using op-amp

                     Application wise:
                 ❖   Low voltage BGR
                 ❖   Low power BGR
                 ❖   High PSRR and low noise BGR
                 ❖   Curvature compared BGR

                   Advantages:
                 ❖ Simplest Technology
                 ❖ Easy to design
                 ❖ Always stable

                     Disadvantages:
                 ❖   Low PSRR
                 ❖   Cascode design needed to reduce PSRR
                 ❖   Voltage head room issue
                 ❖   Need start-up circuit

                     Components:
                 ❖   CTAT voltage generation circuit
                 ❖   PTAT voltage generation circuit
                 ❖   Self-biased current mirror circuit
                 ❖   Reference branch circuit
                 ❖   Start-up circuit
Components Design:
   1. CTAT voltage generation Circuit:




                                   Fig 4. CTAT block diagram, graph and equation

          •   Due to process variation we use BJT instead of a diode.
          •   I0 = Initial current   Is = reverse saturation current
          •   Temperature changes Is changes, Vt changes.
          •   Practically slope will be around -1.5 to -1.6 mV/°C
•   If we increase current through a BJT slope of the BE voltage increase negatively with temperature and
    also slope increase negatively with decrease in Currents.
Lab Outputs:




                                 Fig 5. Single PNP transistor and slope




                                Fig 6. Multiple PNP transistors(m=8) and slope




                     Fig 7. Variable current with single PNP transistor and slope
Observations:
        Slope of single PNP transistor = -1.75mV/°C
        Slope of multiple PNP transistors(m=8) = -1.94mV/°C

    •   In fig 5 u can observe slope is less negative compared to fig 6.
    •   In fig 7 u can observe slope increases negatively with decrease in currents.
2. PTAT voltage generation Circuit:




                         Fig 8. PTAT general block diagram, graph and equation
                V= combined voltage across R1 and Q2 (CTAT in nature but less sloppy)
                         V1 = voltage across Q2(CTAT in nature but more sloppy)
                                V-V1 = voltage across R1 (PTAT in nature)

           •   As u can observe in the fig8 voltage across V1 is more negative as parallel current is divided so
               more sloppy.
     Design of R1:

           •   R1 depends on I floating in circuit that depends on power consumption and silicon area budget.
           •   R1 = Vt* ln(N)/I
           •   Resistance decreases, area decreases with increase in circuit current.
           •   Resistance increases, area increases with decrease in circuit current.
           •   Also, resistance values depend on number of BJTs used in branch 2.
           •   I.e., for 10uA current with N=8, R1 can be calculated as 5.4K ohm.
    Spice simulations:




                         Fig9. PTAT voltage generation circuit simulation graph
Fig10. Current through Vid1 and Vid2 branches simulation




             Fig11. V(qp1) simulation graph




        Fig12. v(qp2) and v(ra1) simulation graph
                                Fig13. Difference between ra1 and qp2 voltages
         Observations from simulations:

               •   In fig9 we can observe that voltage at qp1 and ra1 are same.
               •   In fig10 current in both the branches vid1 and vid2 are same so its satisfying both our theoretical
                   assumption and design specifications.
               •   In fig 12u can see that qp2 has more negative slope than ra1. So, if we observe the difference in
                   fig13 we can observe that slope is positively increasing.


3. Self -biased current mirror circuit:
   The main objective of reference generation is to establish a dc voltage of current which is
       • Independent of supply.
       • Independent of process and
       • Well defined behaviour with temperature.
    Issues:

        •     Output current of this circuit is quite sensitive to Vdd.
        •     For a less sensitive solution, the circuit must bias itself, i.e., Iref must be some how derived from
              Iout. In order to overcome these issues, we are using the below mentioned self- biased current mirror.




                                              Fig14. Self- biased current mirror circuit

        •     The idea of using fig14 is that, Iout is ultimately independent as Iref can be a replica of Iout.
        •     Mp1 and Mp2 copy Iout and define Iref (Iref is bootstrapped to Iout)
        •     Since each diode connected device feeds from a current source, Iout and Iref are relatively
              independent of Vdd.
Issues:

    •     The above circuit is governed by one equation Iout = Iref*K, hence can support any amount of
          current.
    •     To uniquely define the currents another constraint must be added to this circuit.




Updated current mirror circuit:

    •     In this circuit Rs defines current uniquely.
    •     Both Iout and Iref are very little dependent on Vdd.
    •     Loop gain is always less than 1, by default circuit is always stable.




Issues:

    •     An important issue is supply independent biasing is the existence of degenerative bias point.
    •     Strat-up problem when supply turned ON.
4. Reference voltage branch circuit:




                                           Fig15. Reference branch circuit
       •    Current I3 is same as I1 and I2.
       •    Voltage across Q3 is CTAT type. Voltage across R2 is PTAT type.
       •    Vref is the addition of CTAT and PTAT voltage.
       •    R2 = R1*α
    Design of R2 resistance:

        •   Temp-co of Vref should be zero.
        •   As all the values are known, α can be calculated easily.

5. Start-up circuit:




                                         Fig16. Start-up circuit schematic

       •    If channel length modulation is negligible, the current hardly depends on supply voltage.
       •    The main issue is supply independent biasing is the existence of degenerative bias points.
       •    There are 2 stable points.
            1. Iin = Iout = 0 (undesired operating point)
            2. Desired operating point
       •    Must keep the circuit out of undesired point when the supply is turned On.
       •    Must not interfere with the circuit once it reaches the desired operating point.
       •    Initially circuit current is 0. Net2 follows Vdd.
       •    When net2 voltage, vt is more than net6 voltage, current will flow through Mp5 and net1 node up the
            voltage.
6. Complete BGR circuit:




                                       Fig17. BGR circuit schematic
    Simulation outputs:




                               Fig18. BGR using ideal op-amp VCVS




                           Fig19.BGR using ideal op-amp qp1 and qp2 graph plots
Fig20.BGR using ideal op-amp reference voltage – qp3 volatge graph plots




             Fig21.BGR using ideal op-amp qp3 graph plots




         Fig22.BGR using ideal op-amp ra1 and qp2 graph plots
                     Fig23. All plots of BGR using ideal opamp




       Fig24. All plots of BGR using self-biased current mirror for SS corner




Fig25. All branch currents plot of BGR using self-biased current mirror for FF corner
    Fig26. All voltage plots of BGR using self-biased current mirror for FF corner




    Fig27. All voltage plots of BGR using self-biased current mirror for SS corner




Fig28. All branches current plot of BGR using self-biased current mirror for TT corner
Fig29. Self-biased current mirror reference voltage plot for TT corner @1.8V




Fig30. Self-biased current mirror reference voltage plot for FF corner @1.8V




Fig31. Self-biased current mirror reference voltage plot for SS corner @1.8V
Device Data-Sheet:
Design of BGR:
Final Circuit:




LAYOUT:
   1. Design of RESBANK




   2. Design of PFETs:
      We have created a PFETs block by putting all the pfets together, with matching arrangement, also added the
      guard-ring.
3. Design of NFETs:




4. Design of PNP10:




5. Top level layout design:
                                                Appendix:
Spice files:
1. CTAT: Single bjt transistor

   **** ctat voltage generation circuit *****


   .lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
   .include "/home/manda/cad_vsd/eda-
   technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

   .global vdd gnd
   .temp 27

   *** bjt definition
   xqp1         gnd     gnd    qp1      gnd     sky130_fd_pr__pnp_05v5_W3p40L3p40             m=1

   *** supply voltage and current
   vsup       vdd      gnd     dc       1.8
   isup       vdd      qp1     dc       10u
   .dc temp -40        125     5

   *** control statement
   .control
   run
   plot v(qp1)
   .endc
   .end

   Multiple bjt transistor:
   **** ctat voltage generation circuit with multiple bjt *****

   .lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
   .include "/home/manda/cad_vsd/eda-
   technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

   .global vdd gnd
   .temp 27

   *** bjt definition
   xqp1         gnd     gnd    qp1      gnd     sky130_fd_pr__pnp_05v5_W3p40L3p40             m=8

   *** supply voltage and current
   vsup       vdd      gnd     dc       1.8
   isup       vdd      qp1     dc       10u
   .dc temp -40        125     5

   *** control statement
   .control
   run
   plot v(qp1)
   .endc
   .end
  Variable current:
  **** ctat voltage generation circuit with variable current *****

  .lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
  .include "/home/manda/cad_vsd/eda-
  technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

  .global vdd gnd
  .temp 27

  *** bjt definition
  xqp1         gnd     gnd    qp1     gnd      sky130_fd_pr__pnp_05v5_W3p40L3p40             m=1

  *** supply current
  vsup        vdd    gnd      dc      1.8
  isup        vdd    qp1      dc      10u
  .dc temp -40       125      5       isup     1.25u   10u     1.25u

  *** control statement
  .control
  run
  plot v(qp1)
  .endc
  .end

2. PTAT:

  **** ptat voltage generation circuit *****

  .lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
  .include "/home/manda/cad_vsd/eda-
  technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

  .global vdd gnd
  .temp 27

  *** vcvs definition
  e1 net2    gnd      ra1   qp1  gain=1000
  xmp1 q1 net2        vdd   vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4
  xmp2 q2             net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4

  *** bjt definition
  xqp1         gnd   gnd      qp1      vdd    sky130_fd_pr__pnp_05v5_W3p40L3p40              m=1
  xqp2 gnd gnd qp2            vdd    sky130_fd_pr__pnp_05v5_W3p40L3p40     m=8

  *** high-poly resistance definition
  xra1 ra1 na1 vdd sky130_fd_pr__res_high_po_1p41                    w=1.41   l=7.8
  xra2 na1 na2 vdd sky130_fd_pr__res_high_po_1p41                    w=1.41   l=7.8
  xra3 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41                    w=1.41   l=7.8
  xra4 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41                    w=1.41   l=7.8

  *** voltage sources for current measurement
  vid1        q1      qp1      dc     0
  vid2        q2      ra1      dc     0

  *** supply voltage
   vsup        vdd       gnd   dc       1.8
   .dc temp    -40       125   5

   *** control statement
   .control
   run
   plot v(qp1) v(ra1) v(qp2) v(net2)
   plot vid1#branch vid2#branch
   .endc
   .end

   PTAT ideal current source:
   **** ptat voltage generation circuit *****

   .lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
   .include "/home/manda/cad_vsd/eda-
   technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

   .global vdd gnd
   .temp 27


   *** bjt definition
   xqp2 gnd gnd          qp2   vdd     sky130_fd_pr__pnp_05v5_W3p40L3p40       m=8

   *** high-poly resistance definition
   xra1 ra1 na1 vdd sky130_fd_pr__res_high_po_1p41              w=1.41      l=7.8
   xra2 na1 na2 vdd sky130_fd_pr__res_high_po_1p41              w=1.41      l=7.8
   xra3 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41              w=1.41      l=7.8
   xra4 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41              w=1.41      l=7.8

   *** supply voltage and current
   isup       vdd      ra1     dc       10u
   vsup       vdd      gnd     dc       1.8
   .dc temp -40        125     1

   *** control statement
   .control
   run
   plot v(ra1) v(qp2) v(ra1)-v(qp2)
   .endc
   .end


3. BGR using ideal op-amp (VCVS):
   **** bgr using ideal opamp (vcvs) *****


   .lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
   .include "/home/manda/cad_vsd/eda-
   technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

   .global vdd gnd
   .temp 27

   *** vcvs definition
e1 net2 gnd ra1 qp1 gain=1000


xmp1       qp1 net2  vdd  vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4
xmp2       ra1 net2  vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4
xmp3       ref net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4

*** bjt definition
xqp1         gnd   gnd         qp1     gnd    sky130_fd_pr__pnp_05v5_W3p40L3p40   m=1
xqp2 gnd gnd qp2               gnd   sky130_fd_pr__pnp_05v5_W3p40L3p40     m=8
xqp3 gnd gnd qp3               gnd   sky130_fd_pr__pnp_05v5_W3p40L3p40     m=1

*** high-poly resistance definition
xra1 ra1 na1 vdd sky130_fd_pr__res_high_po_1p41               w=1.41   l=7.8
xra2 na1 na2 vdd sky130_fd_pr__res_high_po_1p41               w=1.41   l=7.8
xra3 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41               w=1.41   l=7.8
xra4 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41               w=1.41   l=7.8

xrb1    ref nb1 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb2    nb1 nb2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb3    nb2 nb3 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb4    nb3 nb4 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb5    nb4 nb5 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb6    nb5 nb6 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb7    nb6 nb7 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb8    nb7 nb8 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb9    nb8 nb9 vdd sky130_fd_pr__res_high_po_1p41 w=1.41              l=7.8
xrb10    nb9 nb10 vdd sky130_fd_pr__res_high_po_1p41 w=1.41            l=7.8
xrb11    nb10 nb11 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb12    nb11 nb12 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb13    nb12 nb13 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb14    nb13 nb14 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb15    nb14 nb15 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb16    nb15 nb16 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb17    nb16 nb17 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb18    nb17 nb18 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb19    nb18 nb19 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb20    nb19 nb20 vdd sky130_fd_pr__res_high_po_1p41 w=1.41            l=7.8
xrb21    nb20 nb21 vdd sky130_fd_pr__res_high_po_1p41 w=1.41           l=7.8
xrb22    nb21 nb22 vdd sky130_fd_pr__res_high_po_1p41 w=1.41            l=7.8
xrb23    nb22 qp3 vdd sky130_fd_pr__res_high_po_1p41 w=1.41            l=7.8
xrb24    nb22 qp3 vdd sky130_fd_pr__res_high_po_1p41 w=1.41            l=7.8

*** voltage source for current measurement

*** supply voltage
vsup vdd gnd dc              1.8
*.dc vsup 0        3.3      0.3.3

.dc temp     -40   125     5

*vsup vdd gnd            pulse 0     2   10n   1u   1u   1m    100u
*.tran 5n 10u

.control
RUN
   plot v(vdd) v(qp1) v(ra1) v(qp2) v(ref) v(qp3)
   plot v(ref)


   .endc
   .end


4. BGR using SBCM:

   TT corner :
   **** bandgap reference circuit using self-bias current mirror *****

   .lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
   .include "/home/manda/cad_vsd/eda-
   technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

   .global vdd gnd
   .temp 27

   *** circuit definition ***

   *** mosfet definitions self-biased current mirror and output branch
   xmp1       net1     net2     vdd     vdd     sky130_fd_pr__pfet_01v8_lvt l=2 w=5           m=4
   xmp2 net2 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5                   m=4
   xmp3 net3 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4
   xmn1 net1 net1 q1 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8
   xmn2 net2 net1 q2 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8

   *** start-upcircuit
   xmp4 net4 net2        vdd vdd      sky130_fd_pr__pfet_01v8_lvt        l=2    w=5     m=1
   xmp5 net5 net2        net4 vdd     sky130_fd_pr__pfet_01v8_lvt        l=2    w=5     m=1
   xmp6 net7 net6        net2 vdd     sky130_fd_pr__pfet_01v8_lvt        l=2    w=5     m=2
   xmn3 net6 net6        net8 gnd     sky130_fd_pr__nfet_01v8_lvt        l=7    w=1     m=1
   xmn4 net8 net8        gnd gnd      sky130_fd_pr__nfet_01v8_lvt        l=7    w=1     m=1

   *** bjt definition
   xqp1         gnd   gnd       qp1     vdd    sky130_fd_pr__pnp_05v5_W3p40L3p40              m=1
   xqp2 gnd gnd qp2             vdd   sky130_fd_pr__pnp_05v5_W3p40L3p40     m=8
   xqp3 gnd gnd qp3             vdd   sky130_fd_pr__pnp_05v5_W3p40L3p40     m=1

   *** high-poly resistance definition
   xra1       ra1      na1     vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
   xra2       na1 na2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
   xra3 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
   xra4 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8

   xrb1    vref   nb1    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41      l=7.8
   xrb2    nb1    nb2    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
   xrb3    nb2    nb3    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
   xrb4    nb3    nb4    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
   xrb5    nb4    nb5    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
   xrb6    nb5    nb6    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
   xrb7    nb6    nb7    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
   xrb8    nb7    nb8    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
   xrb9    nb8    nb9    vdd    sky130_fd_pr__res_high_po_1p41     w=1.41       l=7.8
xrb10    nb9    nb10    vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb11    nb10   nb11     vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb12    nb11   nb12     vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb13    nb12   nb13     vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb14    nb13   nb14     vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb15    nb14   nb15     vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb16    nb15   nb16     vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb17    nb16   qp3          vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb18    nb16   qp3     vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8

*** voltage source for current measurement
vid1        q1      qp1     dc      0
vid2 q2 ra1 dc 0
vid3 net3 vref dc 0
vid4 net7 net1      dc      0
vid5        net5    net6    dc      0

*** supply voltage
vsup       vdd     gnd       dc       1.8
*.dc       vsup 0            3.3      0.3.3
.dc temp -40       125       5

*vsup       vdd        gnd   pulse    0       2      10n     1u      1u      1m     100u
*.tran      5n         10u

.control
run

plot v(vdd) v(net1) v(net2) v(qp1) v(ra1) v(qp2) v(vref) v(qp3)
plot vid1#branch vid2#branch vid3#branch vid4#branch vid5#branch

.endc
.end

FF corner:
**** bandgap reference circuit using self-biase current mirror at ff corner*****

.lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice ff"
.include "/home/manda/cad_vsd/eda-
technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

.global vdd gnd
.temp 27

*** circuit definition ***

*** mosfet definitions self-biased current mirror and output branch
xmp1       net1     net2     vdd     vdd     sky130_fd_pr__pfet_01v8_lvt l=2 w=5           m=4
xmp2 net2 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5                   m=4
xmp3 net3 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4
xmn1 net1 net1 q1 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8
xmn2 net2 net1 q2 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8

*** start-upcircuit
xmp4 net4 net2 vdd vdd               sky130_fd_pr__pfet_01v8_lvt     l=2   w=5     m=1
xmp5 net5 net2 net4 vdd              sky130_fd_pr__pfet_01v8_lvt     l=2   w=5     m=1
xmp6     net7 net6 net2 vdd          sky130_fd_pr__pfet_01v8_lvt   l=2   w=5   m=2
xmn3     net6 net6 net8 gnd          sky130_fd_pr__nfet_01v8_lvt   l=7   w=1   m=1
xmn4     net8 net8 gnd gnd           sky130_fd_pr__nfet_01v8_lvt   l=7   w=1   m=1

*** bjt definition
xqp1         gnd   gnd      qp1        vdd    sky130_fd_pr__pnp_05v5_W3p40L3p40        m=1
xqp2 gnd gnd qp2            vdd      sky130_fd_pr__pnp_05v5_W3p40L3p40     m=8
xqp3 gnd gnd qp3            vdd      sky130_fd_pr__pnp_05v5_W3p40L3p40     m=1

*** high-poly resistance definition
xra1       ra1      na1     vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra2       na1 na2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra3 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra4 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8

xrb1     vref   nb1 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb2     nb1    nb2 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb3     nb2    nb3 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb4     nb3    nb4 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb5     nb4    nb5 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb6     nb5    nb6 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb7     nb6    nb7 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb8     nb7    nb8 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb9     nb8    nb9 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb10    nb9     nb10 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb11    nb10    nb11 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb12    nb11    nb12 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb13    nb12    nb13 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb14    nb13    nb14 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb15    nb14    nb15 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb16    nb15    nb16 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb17    nb16    qp3         vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb18    nb16    qp3 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8

*** voltage source for current measurement
vid1        q1      qp1     dc      0
vid2 q2 ra1 dc 0
vid3 net3 vref dc 0
vid4 net7 net1      dc      0
vid5        net5    net6    dc      0

*** supply voltage
vsup       vdd     gnd       dc       1.8
*.dc       vsup 0            3.3      0.3.3
.dc temp -40       125       5

*vsup       vdd     gnd      pulse    0       2     10n    1u      1u     1m    100u
*.tran      5n      10u

.control
run
plot v(vdd) v(net1) v(net2) v(qp1) v(ra1) v(qp2) v(vref) v(qp3)
plot vid1#branch vid2#branch vid3#branch vid4#branch vid5#branch

.endc
.end
SS corner:
**** bandgap reference circuit using self-biase current mirror at ss corner****


.lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice ss"
.include "/home/manda/cad_vsd/eda-
technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

.global vdd gnd
.temp 27

*** circuit definition ***

*** mosfet definitions self-biased current mirror and output branch
xmp1       net1     net2     vdd     vdd     sky130_fd_pr__pfet_01v8_lvt l=2 w=5           m=4
xmp2 net2 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5                   m=4
xmp3 net3 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4
xmn1 net1 net1 q1 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8
xmn2 net2 net1 q2 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8

*** start-upcircuit
xmp4 net4 net2        vdd vdd      sky130_fd_pr__pfet_01v8_lvt      l=2    w=5    m=1
xmp5 net5 net2        net4 vdd     sky130_fd_pr__pfet_01v8_lvt      l=2    w=5    m=1
xmp6 net7 net6        net2 vdd     sky130_fd_pr__pfet_01v8_lvt      l=2    w=5    m=2
xmn3 net6 net6        net8 gnd     sky130_fd_pr__nfet_01v8_lvt      l=7    w=1    m=1
xmn4 net8 net8        gnd gnd      sky130_fd_pr__nfet_01v8_lvt      l=7    w=1    m=1

*** bjt definition
xqp1         gnd   gnd       qp1     vdd    sky130_fd_pr__pnp_05v5_W3p40L3p40              m=1
xqp2 gnd gnd qp2             vdd   sky130_fd_pr__pnp_05v5_W3p40L3p40     m=8
xqp3 gnd gnd qp3             vdd   sky130_fd_pr__pnp_05v5_W3p40L3p40     m=1

*** high-poly resistance definition
xra1       ra1      na1     vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra2       na1 na2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra3 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra4 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8

xrb1    vref   nb1 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb2    nb1    nb2 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb3    nb2    nb3 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb4    nb3    nb4 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb5    nb4    nb5 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb6    nb5    nb6 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb7    nb6    nb7 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb8    nb7    nb8 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb9    nb8    nb9 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb10   nb9     nb10 vdd       sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb11   nb10    nb11 vdd        sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb12   nb11    nb12 vdd        sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb13   nb12    nb13 vdd        sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb14   nb13    nb14 vdd        sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb15   nb14    nb15 vdd        sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb16   nb15    nb16 vdd        sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb17   nb16    qp3           vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb18   nb16    qp3 vdd        sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
*** voltage source for current measurement
vid1        q1      qp1     dc      0
vid2 q2 ra1 dc 0
vid3 net3 vref dc 0
vid4 net7 net1      dc      0
vid5        net5    net6    dc      0

*** supply voltage
vsup       vdd     gnd       dc       1.8
*.dc       vsup 0            3.3      0.3.3
.dc temp -40       125       5

*vsup       vdd       gnd    pulse    0       2     10n     1u         1u     1m    100u
*.tran      5n        10u

.control
run

plot v(vdd) v(net1) v(net2) v(qp1) v(ra1) v(qp2) v(vref) v(qp3)
plot vid1#branch vid2#branch vid3#branch vid4#branch vid5#branch

.endc
.end

Variable supply at TT corner:
**** bandgap reference circuit using self-biase current mirror *****


.lib "/home/manda/cad_vsd/eda-technology/sky130/models/spice/models/sky130.lib.spice tt"
.include "/home/manda/cad_vsd/eda-
technology/sky130/models/spice/models/sky130_fd_pr__model__pnp.model.spice"

.global vdd gnd
.temp 27

*** circuit definition ***

*** mosfet definitions self-biased current mirror and output branch
xmp1       net1     net2     vdd     vdd     sky130_fd_pr__pfet_01v8_lvt l=2 w=5           m=4
xmp2 net2 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5                   m=4
xmp3 net3 net2 vdd vdd sky130_fd_pr__pfet_01v8_lvt l=2 w=5 m=4
xmn1 net1 net1 q1 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8
xmn2 net2 net1 q2 gnd sky130_fd_pr__nfet_01v8_lvt l=1 w=5 m=8

*** start-upcircuit
xmp4 net4 net2         vdd vdd       sky130_fd_pr__pfet_01v8_lvt       l=2   w=5   m=1
xmp5 net5 net2         net4 vdd      sky130_fd_pr__pfet_01v8_lvt       l=2   w=5   m=1
xmp6 net7 net6         net2 vdd      sky130_fd_pr__pfet_01v8_lvt       l=2   w=5   m=2
xmn3 net6 net6         net8 gnd      sky130_fd_pr__nfet_01v8_lvt       l=7   w=1   m=1
xmn4 net8 net8         gnd gnd       sky130_fd_pr__nfet_01v8_lvt       l=7   w=1   m=1

*** bjt definition
xqp1         gnd   gnd       qp1       vdd    sky130_fd_pr__pnp_05v5_W3p40L3p40            m=1
xqp2 gnd gnd qp2             vdd     sky130_fd_pr__pnp_05v5_W3p40L3p40     m=8
xqp3 gnd gnd qp3             vdd     sky130_fd_pr__pnp_05v5_W3p40L3p40     m=1
*** high-poly resistance definition
xra1       ra1      na1     vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra2       na1 na2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra3 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xra4 na2 qp2 vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8

xrb1     vref   nb1 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb2     nb1    nb2 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb3     nb2    nb3 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb4     nb3    nb4 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb5     nb4    nb5 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb6     nb5    nb6 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb7     nb6    nb7 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb8     nb7    nb8 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb9     nb8    nb9 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb10    nb9     nb10 vdd     sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb11    nb10    nb11 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb12    nb11    nb12 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb13    nb12    nb13 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb14    nb13    nb14 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb15    nb14    nb15 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb16    nb15    nb16 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb17    nb16    qp3         vdd sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8
xrb18    nb16    qp3 vdd      sky130_fd_pr__res_high_po_1p41 w=1.41 l=7.8

*** voltage source for current measurement
vid1        q1      qp1     dc      0
vid2 q2 ra1 dc 0
vid3 net3 vref dc 0
vid4 net7 net1      dc      0
vid5        net5    net6    dc      0

*** supply voltage
vsup       vdd     gnd       dc      1.8
.dc vsup 0         3.3       0.1
*.dc       temp -40          125     5

*vsup       vdd     gnd      pulse   0       2   10n    1u         1u   1m   100u
*.tran      5n      10u

.control
run

plot v(vdd) v(net1) v(net2) v(qp1) v(ra1) v(qp2) v(vref) v(qp3)
plot vid1#branch vid2#branch vid3#branch vid4#branch vid5#branch

.endc
.end

