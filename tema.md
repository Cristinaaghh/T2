# T2
Tema 2 SP
%Numar par->semnal triunghiular
%Perioada P=40[sec]
%Durata semnalelor D=2[sec]
%Rezolutia temporara adecvata
%Numarul de coeficienti N=50
%Teoria seriilor Fourier(SFT-trigonometrica, SFA-armonica sau SFC-complexa) arata ca orice semnal periodic poate fi 
%aproximat printr-o suma infinita de sinusui si cosinusuri de frecvente diferite. Fiecare dintre acestea este ponderat 
%cu un anumit coeficient, ce reprezinta practic spectrul. Astfel, semnal reconstruit foloseste un numar infinit de termeni, 
%respectiv 50 in cazul nostru. Acest semnal se apropie ca forma de cel original, dar prezinta o marja de eroare. 
%Daca numarul de coeficienti creste, marja de eroare va fi din ce in ce mai mica. 


P=40;
D=12;
N-50;
w0=2*pi/P;  %pulsatia unghiulara
t_tr=0:0.02:D;  %esantionarea semnalului original
x_tr = sawtooth((pi/12)*t_tr,0.5)/2+0.5;  %semnalul original
t=0:0.02:P;  %esantionarea semnalului modificat
x=zeros(1,length(t)); %initializarea semnalului modificat cu valoril nule
x(t<=D) = x_tr;  %modificam valorile nule cu valori din semnalul original acolo unde avem valori din t<=durata semnalului
figure(1);
plot(t,x), title('x(t)(linie solida) si reconstructia folosind N coeficienti (linie punctata) ');  %afisare semnal
Hold on;

for k = -N:N     %k= variabila dupa care se realizeaza suma 
x_t = x_tr;    % semnalul realizat dupa formula data 
x_t = x_t .* exp(-1i*k*w0*t_tr);    %inmultire intre doua matrice
X(k+N+1) = 0;   %initializam cu zero 
for i = 1:length(t_tr)-1 
X(k+N+1) = X(k+N+1) + (t_tr(i+1)-t_tr(i)) * (x_t(i)+x_t(i+1))/2; %reconstruim folosind coeficientii 
End
end 
for i = 1:length(t)    %i= variabila dupa care se realizeaza suma 
x_finit(i) = 0;   %initializam cu zero 
for k=-N:N 
x_finit(i) = x_finit(i) + (1/P) * X(k+51) * exp(1i*k*w0*t(i)); %reconstructia folosind coeficientii 
end
end
plot(t,x_finit,'--');    %afisarea reconstructiei semnalului cu cei 50 de coeficienti
figure(2);
w=-50*w0:w0:50*w0;    %w este vectorul ce ne va permite afisarea spectrului functiei
stem(w/(2*pi),abs(X)),title('Spectrul lui x(t)'); %afisam spectrul
