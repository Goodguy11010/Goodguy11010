- 👋 Hi, I’m @Goodguy11010
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Goodguy11010/Goodguy11010 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
%mechanical vibaration and measurement spring 2018
% Dynamic modelling of structures using finite elements
%multi- storey building
% Name:  
% student Id:
clear all
clc
 
%definition of parameters
n=8;    %number of elements
m=2*n+2;  %total degree of freedoms(DOF)
rho=7640;
A=5.64e-4;
I=1.1161e-7;
E=207E9;
Lt=2;  %total length of tower
L=Lt/n;
Me=3.96;
%% The global Mass and Stiffness Matrix
%stiffness and mass element matrix
KO=[12 6*L -12 6*L;
    6*L 4*L^2 -6*L 2*L^2;
    -12 -6*L 12 -6*L;
    6*L 2*L^2 -6*L 4*L^2];
MO=[156 22*L 54 -13*L;
    22*L 4*L^2 13*L -3*L^2;
    54 13*L 156 -22*L;
    -13*L -3*L^2 -22*L 4*L^2];
Ktot=zeros(m,m);
for i=1:n
    K=zeros(m,m);
    K(2*i-1:2*i+2,2*i-1:2*i+2)=KO;
    Ktot=Ktot+K; %addition of all stiffness matrix for all elements
end
Mtot=zeros(m,m); %building dofxdof zero matrix
for i=1:n
    M=zeros(m,m);
    M(2*i-1:2*i+2,2*i-1:2*i+2)=MO;
    Mtot=Mtot+M; %addition of all mass matrix for all elements
end
 
Ktot=E*I/(L^3)*Ktot;
Mtot=rho*A*L/420*Mtot;
 
%adding extra mass
for i=1:n
    Mtot(2*i+1,2*i+1)=Mtot(2*i+1,2*i+1)+Me;
end
%% application of boundary condition
Ktot(:,[1,2])=[];
Ktot([1,2],:)=[];
Mtot(:,[1,2])=[];
Mtot([1,2],:)=[];
%% building system matrix
S=inv(Mtot)*Ktot;
[Vectors, Values]=eig(S);%eigenvalue/eigenvectors
for ii=1:m-2
    wn(ii)=abs(sqrt(Values(ii,ii)));
    maxvect(ii)=max(abs(Vectors(1:m-2,ii)));
    for jj=1:m-2;
        mdshp(jj,ii)=Vectors(jj,ii)/maxvect(ii);
    end
end
%% sorting the order of the frequencies and shapes
 
[wn, r]=sort(wn);
for kk=1:m-2
    mdshpsort(:,kk)=mdshp(:,r(kk));
end
mdshp=mdshpsort;
%%elimitating column 9 to 16
mdshp(:,[9:16])=[]
for ii=2:n+1
    mdshp(ii,:)=[];
end
%%normalizing one more time after eliminating even rows
for ii=1:8
    maxvect2(ii)=max(abs(mdshp(1:8,ii)));
    for jj=1:8;
        mdshp(jj,ii)=mdshp(jj,ii)/maxvect2(ii);
    end
end
% where wn is the output natural frequency and mshp is output normalised
% mode shapes
%% display mode shapes
y=(0:1:8); % y values
B=[0 0 0 0 0 0 0 0];
mdshp=[B;mdshp];
%plot of each mode shape
figure (1);
subplot (1,8,1)
plot (mdshp(:,1),y);
title('mode 1');
subplot(1,8,2);
plot (mdshp(:,2),y);
title('mode 2');
subplot(1,8,3);
plot (mdshp(:,3),y);
title('mode 3');
subplot(1,8,4);
plot (mdshp(:,4),y);
title('mode 4');
subplot(1,8,5);
plot (mdshp(:,5),y);
title('mode 5');
subplot(1,8,6);
plot (mdshp(:,6),y);
title('mode 6');
subplot(1,8,7);
plot (mdshp(:,7),y);
title('mode 7');
subplot(1,8,8);
plot (mdshp(:,8),y);
title('mode 8');
% plot of first five mode shapes
figure (2);
plot (mdshp(:,1:5),y)
title ('first five theoritical mode shapes')
![image](https://user-images.githubusercontent.com/119382973/204425587-ab40ffe6-f180-4bea-b3d2-19f0a892a4e3.png)
