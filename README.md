# PBPK-enrofloxacin1
## Barekely Madona codes for seven caompartments BPPK model for enrofloxacin in calves
### 7 compartments: liver, kidney, lungs, muscle, fat, intestine and the rest of the body
#### Two submodels: parent drug/enrofloxacin (seven compartments) and metabolite/ciprofloxacin (muscle and fat excluded)

METHOD RK4

STARTTIME = 0
STOPTIME = 50 # Data of plasma and fecal samples collected upto 48 hrs is available for model validation
DT = 0.001
DTOUT = 0.1

; Physiological parameters

; Tissue volumes
BW = 118 ; Body weight (kg) (median of 35 calves in FQ AMR cattle study, ISU)
;VAC = 0.0104 ;    Fractional arterial blood (Lin et al., 2016 SS Table 2, update by original references); plasma?
;VVC = 0.0296 ;    Fractional venous blood (Lin et al., 2016 SS Table 2, update by original references)
VbloodC = 0.0691 ; Fractional Blood volume, Is VbloodC different from VAC/VVC? IS it plasma? (Lin et al., 2020; Table 7)
VLC = 0.0287 ;     Fractional liver tissue (Lin et al., 2020; Table 7)
VKC = 0.0039 ;     Fractional kidney tissue (Lin et al., 2020; Table 7)
VFC = 0.0695 ;     Fractional fat tissue (Lin et al., 2020; Table 7)
VMC = 0.339 ;      Fractional muscle tissue (Lin et al., 2020; Table 7)
VLuC = 0.0123 ;    Fractional lung tissue in calves (Lin et al., 2020; Table 7)
VIC = 0.0239 ;     Fractional intestine volumes in calves (Lin et al., 2020; Table 7; the whole intestine or LI alone?)
VRC = 0.4536 ;     Fractional rest of body (1-VLC-VKC-VFC-VMC-VLuC-VIC-VbloodC)

; Blood flow rates
QCC = 9.09 ; cardiac output (L/h/kg) (Lin et al., 2020; Table 16)

;Fraction of blood flow to organs (unitless, percent)
;QLC = 0.30 ;  Fraction of blood flow to the liver (Lin et al., 2020; Table 24; Hepatic artery = 0.04 and Portal vein = 0.28)
QLhC = 0.04 ;  Fraction of blood flow to the liver hepatic artery (Lin et al., 2020; Table 24)
QLpC = 0.28 ;  Fraction of blood flow to the liver portal vein (from intestine, Lin et al., 2020; Table 24)
QKC = 0.10 ;   Fraction of blood flow to the kidneys (Lin et al., 2020; Table 24)
QFC = 0.08 ;   Fraction of blood flow to the fat (Lin et al., 2016 SS Table 2, search for calves)
QMC = 0.18 ;   Fraction of blood flow to the muscle (Lin et al., 2016 SS Table 2, search for calves)
QLuC = 0.46 ;  Fraction of blood flow to the lungs (Lin et al., 2020; Table 24);
QIC = 0.11 ;   Fraction of blood flow to the GI tract (Lin et al., 2020; Table 24, search for intestines alone)
QRC = 0.21 ;   Fraction of blood flow to the rest of the body (1-QLhC-QLpC-QKC-QFC-QMC-QGC)

; Mass Transer parameters (Chemical-specific parameters)
; partition coefficients (PC, tissue:plasma)
; Parent drug, enrofloxacin
PL = 4.3 ;   Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK = 5.5 ;   Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF = 0.53 ;  Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM = 1.09 ;  Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu = 4.3 ;  Lung:plasma PC (Lin et al., 2016 SS Table 4)
PI = ? ;     Intestine:plasma
PR = 1.5;    Rest of body:plasma (Lin et al., 2016 SS Table 4) Is PR affected by the number of compartments? e.g. whether GIT is included or not?

; main metabolite, ciprofloxacin
PL1 = 4.3 ;   Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK1 = 5.5 ;   Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF1 = 0.53 ;  Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM1 = 1.09 ;  Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu1 = 4.3 ;  Lung:plasma PC (Lin et al., 2016 SS Table 4)
PI1 = ? ;     Intestine:plasma
PR1 = 1.5     Rest of body:plasma  ; (Lin et al., 2016 SS Table 4)

KmC = 0.06 ; Hepatic metabolic rate [/(h*kg)] (Lin et al., 2016 SS Table 4)

;Kinetic constants
Ksc = 0.06 ;       SC absorpation rate constant (/h) (Lin et al., 2016 SS Table 4)
Frac = 0.55 ;      Fraction of ENR metabolized to CIP (unitless),  (Lin et al., 2016 SS Table 4)
PB = 0.46 ;        ENR percentage of plasma protein binding (unitless)  (Lin et al., 2016 SS Table 4)
PB1 = 0.19 ;       CIP percentage of plasma protein binding (unitless) (Lin et al., 2016 SS Table 4s)
KurineC = 0.15 ;   ENR urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4) 
Kurine1C = 1.99 ;  CIP urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4)

; SC infusion rate constants
Timesc = 0.00 ; SC injection/infusion (h)

; Billiary elimination rate constant??
KfecesC = 0.01 ; ENR L/h/kg (Lin et al., 2016 SS Table 4, 0.01 if for swine)
KfecesC1 = ? ; CIP L/h/kg It was not considered for CIP Lin et al 2016 but for unrinary excretion both ENR and CIP were included.

; Parameters for exposure scenario
PDOSEscLd = 7.5 ; (mg/kg) Low dose scenario 
PDOSEscHd = 12.5 ; (mg/kg) High dose scenario

; Cardiac output and blood flows to tissues (L/h)
QC = QCC*BW ; Cardiac output
QL = QLC*QC ; Liver
QK = QKC*QC ; kidney
QF = QFC*QC ; Fat
QLu = QLuC*QC ; Lung
QM = QMC*QC ; Muscle
QI = QIC*QC ; Intestine
QR = QRC*QC ; Rest of body

; Tissue Volume (L)
VL = VLC*BW ; Liver
VK = VKC*BW ; Kidney
VF = VFC*BW ; Fat
VM = VMC*BW ; Muscle
VLu = VLuC*BW ; Lung
VI = VIC*BW ; Intestine
VR = VRC*BW ; Rest of the body

; Dosing
DOSEscLd = PDOSEscLd*BW ; (mg) (low dose)
DOSEscHd = PDOSEscHd*BW ; (mg) (High dose)

; ENR sc injection to the subcut
; Low dose scenario
SCRLd = DOSEscLd/Timesc; Low dose
RSCLd = SCRLd*(1.-step(1, Timesc)
d/dt(AscLd) = RSCL
init AscLd = 0

; High dose scenario
SCRHd = DOSEscHd/Timesc, High dose
RSCHd = SCRHd*(1.-step(1, Timesc)
d/dt(AscHd) = RSCHd
init AscHd = 0

; Urinary elimination rate constant
Kurine = KurineC*BW ; ENR
Kurine1 = Kurine1C*BW ; CIP

; Fecal elimination rate constant
Kfeces = KfecesC*BW ; ENR
Kfeces1 = Kfeces1C*BW ; CIP

;ENR in blood compartment
CVHd = ((QL*CVL+QK*CVK+QF*CVF+QM*CVM+QLu*CVLu+QR*CVR+QI*CVI+RscHd)/QC ; High dose
CVLd = ((QL*CVL+QK*CVK+QF*CVF+QM*CVM+QLu*CVLu+QR*CVR+QI*CVI+RscLd)/QC ; Low dose

;CIP in blood compartment
CV1Hd = ((QL*CVL+QK*CVK+QF*CVF+QM*CVM+QLu*CVLu+QR*CVR+QI*CVI+RscHd)/QC ; High dose
CV1Ld = ((QL*CVL+QK*CVK+QLu*CVLu+QR*CVR+QI*CVI+RscLd)/QC ; Low dose

;ENR
RA= QC*(CV-CA) ; CV-venous blood, CA- arterial blood
d/dt(AA) = RA
init AA = 0
CA = AA/Vblood ; concetration in the artery

;CIP
RA1= QC*(CV1-CA1) ; CV-venous blood, CA- arterial blood
d/dt(AA) = RA1
init AA = 0
CA1 = AA1/Vblood ; concetration in the artery

; ENR in liver compartment
RL = QL*(CA-CVL) ; Rate of ENR change in liver
d/dt(AL) = RL
init AL = 0
CL = AL/VL ; concetration in the liver (AL - amount in liver, VL - Volume of liver)
CVL = AL/(VL*PL) ; concentration in liver venous blood

; ENRO in kidney compartment
RK = QK*(CA-CVK) ; Rate of ENR change in kideny
d/dt(AK) = RK
init AK = 0
CK = AK/VK ; Concentration in kideny
CVK = AK/(VK*PK) ; Concentration in kideny venous blood

; ENRO in lung compartment
RLu = QLu*(CA-CVLu) ; rate of ENR change in lung
d/dt(ALu) = RLu
init ALu = 0
CLu = ALu/VLu ; Concentration in lung
CVLu = ALu/(VLu*PLu) ; Concentration in lung venous blood

; ENRO in muscle compartment
RM = QM*(CA-CVM) ; Rate of ENR change in muscle
d/dt(AM) = RM
init AM = 0
CM = AM/VM ; Concentration in muscle
CVM = AM/(VM*PM) ; Cocentration in muscle venous blood

; ENRO in fat compartment
RF = QF*(CA-CVF)
d/dt(AF) = RF
init AF = 0
CF = AF/VF
CVF = AF/(VF*PF) ; Concentration venous blood of fat

; ENRO in intestine compartment
RI = QF*(CA-CVI)
d/dt(AI) = RI
init AI = 0
CI = AI/VI ; ENR concentration in intestine
CVI = AI/(VI*PI) ; Concentration in intestinal venous blood

; ENRO in rest of the body
RR = QR*(CA-CVR)
d/dt(AR) = RR
init AR = 0
CR = AR/VR
CVR = AR/(VR*PR) ; Concentration in venous blood of the rest of body

; CIProfloxacin
; CIP in liver compartment
RL1 = QL*(CA1-CVL1) ; Rate CIP change in venous blood
d/dt(AL1) = RL1
init AL1 = 0
CL1 = AL1/VL ; CIP Concentration in liver
CVL1 = AL1/(VL*PL1) ; CIP concentration in liver

; CIPRO in kidney compartment
RK1 = QK*(CA1-CVK1) ; Rate of CIP change in kideny
d/dt(AK1) = RK1
init AK1 = 0
CK1 = AK1/VK ; Concentration in kidney
CVK1 = AK1/(VK*PK1) ; Concentration in kidney venous blood

; CIP in lung compartment
RLu1 = QLu1*(CA1-CVLu1) ; rate of CIP change in lung
d/dt(ALu1) = RLu1
init ALu1 = 0
CLu1 = ALu1/VLu ; CIP Concentration in lung
CVLu1 = ALu1/(VLu*PLu1) ; CIP Concentration in lung venous blood

; CIPRO in intestine compartment
RI1 = QF*(CA1-CVI1)
d/dt(AI1) = RI1
init AI1 = 0
CI1 = AI1/VI ; CIP Concentration in intestinal
CVI1 = AI1/(VI*PI1) ; CIP Concentration in intestinal venous blood

; CIPRO in rest of body compartment
RR1 = QR*(CA1-CVR1) ; Rate of CIP change in rest of body
d/dt(AR1) = RR1
init AR1 = 0
CR1 = AR1/VR ; CIP concentration rest of the body
CVR1 = AR1/(VR*PR1) ; CIP Concentration in venous blood of the rest of body

; Urinary excretion of ENR
Rurine = Kurine*CVK
d/dt(Aurine) Rurine
inti Aurine = 0

; Urinary excretion of CIP
Rurine1 = Kurine1*CVK1
d/dt(Aurine) Rurine
inti Aurine = 0

; For biliary excretion refer to Module 7.3 lecture
; Fecal/biliary? excretion of ENR (The purpose is to know the concentration of ENR and CIP in the intestinal contents)
KbC = ? ; ENR bile elimination rate constant 
Kb = KbC*BW ; ENR elemination rate

; Fecal/biliary excretion of CIP
KbC1 = ? ; CIP bile elimination rate constant 
Kb1 = KbC1*BW ; CIP elimination rate

; Bile excretion of ENR
Rbile = Kb*CVL ; rate of bile excretion of ENR
Abile = integ(Rbile, 0) ; is it the same/similar with Afeces?

; Bile excretion of CIP
Rbile1 = Kb1*CVL ; rate of bile excretion of CIP
Abile1 = integ(Rbile1, 0)

; Mass balance
Qbal = QC-QL-QK-QF-QM-QLu-QI-QR ; cardiac out balance (same for ENR and CIP)
Tmass = AA+AL+AK+AF+AM+ALu+AI+AR+Aurine+Afeces+Abile; ENR amount
BaLd; = AscLd-Tmass ; Mass balance for LOW dose ; ENR low dose
BaHd; = AscHd-Tmass ; Mass balance for HIGH dose ; ENR high dose

Tmass1 = AA1+AL1+AK1+AF1+AM1+ALu1+AI1+AR1+Aurine1+Abile1 ; CIP amount
BaLd1; = AscLd-Tmass1 ; Mass balance for LOW dose CIP
BaHd1; = AscHd-Tmass1 ; Mass balance for HIGH dose CIP









