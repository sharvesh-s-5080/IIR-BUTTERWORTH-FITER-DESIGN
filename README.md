## EXP 3 A : IIR BUTTERWORTH FITER DESIGN

### AIM: 
 To design an IIR Butterworth filter using bilinear transformation in SCILAB. 

### APPARATUS REQUIRED: 
PC installed with SCILAB. 

### PROGRAM (LPF): 
```python
clc;
clear;
close;

// INPUTS
wp = input('Pass band digital frequency (Radians)= ');
ws = input('Stop band digital frequency (Radians)= ');

// Linear gains
Ap = input('Pass band gain (Linear)= ');
As = input('Stop band gain (Linear)= ');
T = input('Sampling Time= ');

// PREWARPING
omegap = (2/T)*tan(wp/2);
omegas = (2/T)*tan(ws/2);

disp("Prewarped omega p = " + string(omegap));
disp("Prewarped omega s = " + string(omegas));

// ORDER
N = (1/2) * log10(((1/(As^2))-1)/((1/(Ap^2))-1)) / log10(omegas/omegap);
disp("Calculated Order N = " + string(N));

N = ceil(N);
disp("Rounded Order N = " + string(N));

// CUTOFF
omegac = omegas / (((1/(As^2))-1)^(1/(2*N)));
disp("Cutoff frequency omega c = " + string(omegac));

// NORMALIZED ANALOG FILTER

Hs_norm = analpf(N, 'butt', [0,0], 1);
disp("Normalized Analog Transfer Function Hn(s) = ");
disp(Hs_norm);


// UNNORMALIZED ANALOG FILTER

Hs = analpf(N, 'butt', [0,0], omegac);
disp("Unnormalized Analog Transfer Function H(s) = ");
disp(Hs);


// BILINEAR TRANSFORMATION

z = poly(0,'z');
Hz = horner(Hs, (2/T)*((z-1)/(z+1)));

disp("Digital Transfer Function H(z) = ");
disp(Hz);


// FREQUENCY RESPONSE
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency (w/pi)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR LPF');

```


### PROGRAM (HPF): 
```python
clc;
clear;
close;

// GIVEN SPECIFICATIONS
T = 0.5;

wp = 0.65*%pi;
ws = 0.45*%pi;

Gp = 0.707;
Gs = 0.2;

// PREWARPING (Bilinear Transformation)
omegap = (2/T)*tan(wp/2);
omegas = (2/T)*tan(ws/2);

disp("Prewarped passband frequency = " + string(omegap));
disp("Prewarped stopband frequency = " + string(omegas));

// FILTER ORDER CALCULATION
N_exact = (1/2)*log10(((1/(Gs^2))-1)/((1/(Gp^2))-1)) / log10(omegap/omegas);
disp("Exact filter order = " + string(N_exact));

N = ceil(N_exact);
disp("Rounded filter order = " + string(N));

// CUTOFF FREQUENCY
omegac = omegap*((1/(Gp^2)-1)^(1/(2*N)));
disp("Cutoff frequency = " + string(omegac));

// ANALOG BUTTERWORTH LPF PROTOTYPE
hs_lpf = analpf(N,"butt",[0,0],1);

// LPF → HPF TRANSFORMATION
s = poly(0,'s');
hs_hpf = horner(hs_lpf, omegac/s);

disp("Analog High Pass Transfer Function H(s)");
disp(hs_hpf);

// BILINEAR TRANSFORMATION
z = poly(0,'z');
Hz = horner(hs_hpf,(2/T)*((z-1)/(z+1)));

disp("Digital High Pass Filter H(z)");
disp(Hz);

// FREQUENCY RESPONSE
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;

plot(w/%pi,abs(HW));
xlabel("Normalized Frequency (w/pi)");
ylabel("Magnitude");
title("Frequency Response of Butterworth HPF");
xgrid();

```


### OUTPUT (LPF) : 
<img width="340" height="451" alt="image" src="https://github.com/user-attachments/assets/75a60249-8d8e-4f10-bb3b-a450ad3402b7" />

<img width="351" height="492" alt="image" src="https://github.com/user-attachments/assets/10f3277c-2a4d-4bba-8649-d594111e4463" />

### OUTPUT (HPF) :
<img width="295" height="406" alt="image" src="https://github.com/user-attachments/assets/bbaca645-a0b8-4d8c-abcd-44bcc0847a60" />

<img width="345" height="493" alt="image" src="https://github.com/user-attachments/assets/51062bc8-25f6-4534-9e7b-f827b984617e" />

### RESULT: 
Thus , the IIR Butterworth filter was designed successfully using bilinear transformation in SCILAB.
