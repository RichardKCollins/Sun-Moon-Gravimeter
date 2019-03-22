# Sun-Moon-Gravimeter
Software in support of Gravimeters able to detect the vector tidal acceleration of the sun and moon

Superconducting gravimeters, broadband seismometers, MEMS gravimeters, atom chip gravimeters, and a wide variety of instruments and experiments are sensitive enough now to have to correct for the changing gravitational potential and/or the associated gradient of the time dependent gravitational potential.  It is the acceleration field (the gradient of the potential) that is detected by the gravimeters and sensitive accelerometers.

The ordinary units of the vector tidal gravity are nanometers per second squared (nm/s2).  You might remember that the vertical gravity is about 9.8 meters per second squared (m/s2) which is 10^9 nm/s2.  The tidal gravity signal from the sun and moon is a vector that sits at the instrument, and points in various directions over time.  It does not point toward the sun or moon, but is a vector calculation.  I find it best to just ignore where the sun and moon are located, and just imaging the changing signal slowly changing direction and strength over time.

To calculate the expected value for a location is fairly simple.  As a first approximation, which fits rather well for low sampling rates, you can use the familiar Newtonian gravity calculation you learned in grade school.  G*M1*M2/r^2.  Since we are only concerned with force per unit mass, the acceleration, we will use

GMs = 1.32712440018E20 m3/s2  (meters cubed per second squared)
GMm = 4.902801E12 m3/s2

You can find the current GMs for the sun at https://ssd.jpl.nasa.gov/?constants
You can find the current GMm for the sun at https://ssd.jpl.nasa.gov/?sat_phys_par

The tidal acceleration from the sun and moon is

g(t) = [GMs/(Rsi^2) - GMs/(Rse^2)] + [GMm/(Rmi^2) - GMm/(Rme^2)]

This looks complicated, but it is easy to remember if you say "The sun acting on the station minus the sun acting on the center of the earth.  To that you add, the moon acting on the station minus the moon acting on the center of the earth.

Rsi is the distance from the center of the sun to the instrument in meters.
Rse is the distance from the center of the sun to the center of the earth in meters.
Rmi is the distance from the center of the moon to the instrument in meters.
Rme is the distance from the center of the moon to the center of the earth in meters.

Rsi = Ris - Rei, where Res is a vector from the center of the earth to the sun, and Rei is a vector from the center of the earth to the station.
Rmi = Rem - Rei

Rse is just the negative of Res

Eventually the network of gravimeters will be used to refine these solar system reference numbers.  More than likely Jet Propulsion Labs will keep pushing their technology to continually refine their model.

A three axis gravimeter of this sensitivity can be used as a kind of gravitational GPS.  Tracking the sun and moon to this level of accuracy allows you to solve for the orientation and location of the instrument which best minimizes the sum of squares of the residuations.  For beginners, just making their first sun moon gravimeter, you might just be happy to see the signal and see that it roughly matches your signal.  The signal varies depending on where you are on the earth, the time of day, time of year and the year. 

You will have to adjust the signal from the sun and moon to account for the rotation of the earth and your local centrifugal acceleration.  This depends on the rate of rotation of the earth, and on the geodetic latitude (more later).  The rotational period of the earth is called the "mean sidereal day":

mean sidereal day = 86164.09054 seconds

The signal is roughly +/- 1500 nm/s2.  It can have its largest value in the North, East or Vertical direction. And that will change as the earth rotates.  This is the part of the measurement you use to "lock on" to and calibrate your instrument.  The rest of your signal, the "residual" will depend on instrument noise, seismic noise, changes in local gravity because of the varying density of the air near to the station, changes in temperature, changes in magnetic and electric fields, changes from cars and trucks going by,  changes from people in the room, from waves at the beach, and much more. It is roughly +/- 50 nm/s2

ResultVector = SunMoonSignalVector + CentrifugalVector

If your instrument has three axes reporting, they will probably be oriented to North, East and Vertical.  You "dot" the ResultVector into the local vertical unit vector to get the vertical part of the signal, dot the ResultVector into the North unit vector to get the North component of the signal, and dot the ResultVector into the East unit vector to get the East component of the signal.

gN = Dot(ResultVector, NorthUnitVector)
gE = Dot(ResultVector, EastUnitVector)
gV = Dot(ResultVector, VerticalUnitVector)

These should only require a linear regression (offset and multiplier) for each axis.  If your instrument is not quite lined up with the North and East and Vertical, then you can use add rotations to correct your orientation.  If you instrument location is not quite correct, you can solve for the latitude, longitude and height that best reduces the sum of squares of the residuals from the regressions.

This is only a beginning.  Just a way to push your instrument to where it can detect this large and stable gravitational signal.
