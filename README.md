# PBPK-enrofloxacin1
## Barekely Madona codes for seven caompartments BPPK model for enrofloxacin in calves
### 8 compartments: liver, kidney, lungs, muscle, fat, plasma, intestine and the rest of the body
#### Two submodels: parent drug/enrofloxacin (seven compartments) and metabolite/ciprofloxacin (muscle and fat excluded)

METHOD RK4

STARTTIME = 0
STOPTIME = 50 # Data of plasma and fecal samples collected upto 48 hrs is available for model validation
DT = 0.001
DTOUT = 0.1

; Physiological parameters
; Blood flow rates
QCC = 9.09 ; cardiac output (L/h/kg) (Lin et al., 2020; Table 16)

;Fraction of blood flow to organs (unitless, percent)
QLC = 0.30 ; Fraction of blood flow to the liver (Lin et al., 2020; Table 24; Hepatic artery = 0.40 and Portal vein = 0.28)
QKC = 0.10 ; Fraction of blood flow to the kidneys (Lin et al., 2020; Table 24)
QFC = 0.08 ; Fraction of blood flow to the fat (Lin et al., 2016 SS Table 2, double check if it is not of calves)
QMC = 0.18 ; Fraction of blood flow to the muscle (Lin et al., 2016 SS Table 2, double check if it is not of calves)
QLuC = 0.46 ; Fraction of blood flow to the lungs (Lin et al., 2020; Table 24);
QGC = 0.11 ; Fraction of blood flow to the GI tract (Lin et al., 2020; Table 24)
QRC = 0.23 ; Fraction of blood flow to the rest of the body (1-QLC-QKC-QFC-QMC-QGC), QLuC is included(?)

; Tissue volumes
BW = 118 ; Body weight (kg) (median of 35 calves in FQ AMR cattle study)
;VAC = 0.0104 ; Fractional arterial blood (Lin et al., 2016 SS Table 2, update by original references); plasma?
;VVC = 0.0296 ; Fractional venous blood (Lin et al., 2016 SS Table 2, update by original references)
VLC = 0.0287 ; Fractional liver tissue (Lin et al., 2020; Table 7)
VKC = 0.0039 ; Fractional kidney tissue (Lin et al., 2020; Table 7)
VFC = 0.0695 ; Fractional fat tissue (Lin et al., 2020; Table 7)
VMC = 0.339 ; Fractional muscle tissue (Lin et al., 2020; Table 7)
VbloodC = 0.0691 ; Blood volume, fraction of BW #??? is VbloodC different from VAC/VVC? IS it plasma? (Lin et al., 2020; Table 7)
VLuC = 0.0123 ; Fractional lung tissue in calves (Lin et al., 2020; Table 7)
VGC = 0.0547 ; Fractional GI tract volumes in calves (Lin et al., 2020; Table 7; stomachs = 2.22, and intestines = 2.39), Intestine alone or the whole GT tract?
VRC = 0.423 ; Fractional rest of body (1-VLC-VKC-VFC-VMC-VLuC-VGC)

; Mass Transer parameters (Chemical-specific parameters)
; partition coefficients
; Parent drug, enrofloxacin
PL = 4.3 ; Liver:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PK = 5.5 ; Kidney:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PF = 0.53 ; Fat:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PM = 0.1.09 ; Muscle:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PLu = 4.3 ; Lung:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PG = ? ; GI tract: plasma
PR = 1.5; Rest of body:plasma (Lin et al., 2016 SS Table 4, update by original references)

; main metabolite, ciprofloxacin
PLm = 4.3 ; Liver:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PKm = 5.5 ; Kidney:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PFm = 0.53 ; Fat:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PMm = 1.09 ; Muscle:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PLum = 4.3 ; Lung:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PGm = ? ; GI tract: plasma
PRm = 1.5 Rest of body:plasma  ; (Lin et al., 2016 SS Table 4, update by original references)
KmCm = 0.06 ; Hepatic metabolic rate [/(h*kg)] (Lin et al., 2016 SS Table 4, update by original references)
;Fraction of enro metabolized to cipro (unitless), Frac = 0.55 (Lin et al., 2016 SS Table 4, update by original references)
;Parent drug  PB = 0.46 (Lin et al., 2016 SS Table 4, update by original references)
;Main metabolite PB1 = 0.19 (Lin et al., 2016 SS Table 4, update by original references)

; SC infusion rate constants
Timesc = 0.00 ; SC injection/infusion (h)

; Urinary elimination rate constant
KurineC = 0.15 ; Enro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)
KurineCm = 1.99, cipro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)

; Billiary elimination rate constant??
KfecesC = ? ; Enro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)
KfecesCm = ? ; cipro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)

; Parameters for exposure scenario
PDOSEscL = 7.5 ; (mg/kg) Low dose scenario (PDOSEscL/low dose=7.5 and PDOSEscH/High dose =12.5?)
PDOSEscH = 12.5 ; (mg/kg) High dose scenario

; Cardiac output and blood flows to tissues (L/h)
QC = QCC*BW ; Cardiac output
QL = QLC*QC ; Liver
QK = QKC*QC ; kidney
QF = QFC*QC ; Fat
QLu = QLuC#QC ; Lung
QM = QMC*QC ; Muscle
QG = QGC*QG ; GI tract
QR = ? ; Rest of th body
; QR = 0.626*QC-QL-QK-QLu ; Richly perfused tissues
; QS = 0.374*QC-QF-QM ; Slowly perfused tissues

; Tissue Volume (L)
VL = VLC*BW ; Liver
VK = VKC*BW ; Kidney
VF = VFC*BW ; Fat
VM = VMC*BW ; Muscle
VLu = VLuC*BW ; Lung
Vblood = VbloodC*BW ; Blood
VG = VGC*BW ; GI tract
VR = VRC*BW ; Rest of the body
; VR = 0.142*BW-VL-VK-VLu ; Richly perfused tissues
; VS = 0.776*BW-VF-VM ; Slowly perfused tissues

; Dosing
DOSEscL = PDOSEscL*BW ; (mg) (low doses)
DOSEscH = PDOSEscH*BW ; (mg) (High doses)

; Enro sc injection to the venous
SCR = DOSEsc/Timesc, Low and high doses?
RSC = SCR*(1.-step(1, Timesc)
d/dt(Asc) = RSC
init Asc = 0

; Urinary elimination rate constant
Kurine = KurineC*BW

; Fecal elimination rate constant
Kfeces = KfecesC*BW

;Enro in blood compartment
CV = ((QL*CVL+QK*CVK+QF*CVF+QM*CVM+QLu*CVLu+QR*CVR+QS*CVS+Rsc)/QC

RA= QC*(CV-CA) ; CV-venous blood, CA- arterial blood
d/dt(AA) = RA
init AA = 0
CA = AA/Vblood

; ENRO in liver compartment
RL = QL*(CA-CVL)
d/dt(AL) = RL
init AL = 0
CL = AL/VL
CVL = AL/(VL*PL)

; ENRO in kidney compartment
RK = QK*(CA-CVK)
d/dt(AK) = RK
init AK = 0
CK = AK/VK
CVK = AK/(VK*PK)

; ENRO in lung compartment
RLu = QLu*(CA-CVLu)
d/dt(ALu) = RLu
init ALu = 0
CLu = ALu/VLu
CVLu = ALu/(VLu*PLu)

; ENRO in muscle compartment
RM = QM*(CA-CVM)
d/dt(AM) = RM
init AM = 0
CM = AM/VM
CVM = AM/(VM*PM)

; ENRO in fat compartment
RF = QF*(CA-CVF)
d/dt(AF) = RF
init AF = 0
CF = AF/VF
CVF = AF/(VF*PF)

; ENRO in GI tract compartment
RG = QF*(CA-CVG)
d/dt(AG) = RG
init AG = 0
CG = AG/VG
CVG = AG/(VG*PG)

; ENRO in rest of the body
RR = QR*(CA-CVR)
d/dt(AR) = RR
init AR = 0
CR = AR/VR
CVR = AR/(VR*PR)


; CIPRO in liver compartment

; CIPRO in kidney compartment

; ENRO in lung compartment

; CIPRO in plasma compartment

; CIPRO in GI tract compartment

; CIPRO in rest of body compartment


; Urinary excretion of ENRO
Rurine = Kurine*CVK
d/dt(Aurine) Rurine
inti Aurine = 0

; Fecal excretion of CIPRO

; Urinary excretion of CIPRO

; Fecal excretion of CIPRO

; Mass balance
Qbal = QC-QL-QK-QF-QM-QLu-QR-QS-QG
Tmass = AA+AL+AK+AF+AM+ALu+AR+AS+Aurine
Ba; = Aiv-Tmass ; Mass balance











