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
;VAC = 0.0104 ; Fractional arterial blood (Lin et al., 2016 SS Table 2, update by original references); plasma?
;VVC = 0.0296 ; Fractional venous blood (Lin et al., 2016 SS Table 2, update by original references)
VbloodC = 0.0691 ; Fractional Blood volume, Is VbloodC different from VAC/VVC? IS it plasma? (Lin et al., 2020; Table 7)
VLC = 0.0287 ; Fractional liver tissue (Lin et al., 2020; Table 7)
VKC = 0.0039 ; Fractional kidney tissue (Lin et al., 2020; Table 7)
VFC = 0.0695 ; Fractional fat tissue (Lin et al., 2020; Table 7)
VMC = 0.339 ; Fractional muscle tissue (Lin et al., 2020; Table 7)
VLuC = 0.0123 ; Fractional lung tissue in calves (Lin et al., 2020; Table 7)
VIC = 0.0239 ; Fractional intestine volumes in calves (Lin et al., 2020; Table 7; the whole intestine or LI alone?)
VRC = 0.4536 ; Fractional rest of body (1-VLC-VKC-VFC-VMC-VLuC-VIC-VbloodC)

; Blood flow rates
QCC = 9.09 ; cardiac output (L/h/kg) (Lin et al., 2020; Table 16)

;Fraction of blood flow to organs (unitless, percent)
;QLC = 0.30 ; Fraction of blood flow to the liver (Lin et al., 2020; Table 24; Hepatic artery = 0.04 and Portal vein = 0.28)
QLhC = 0.04 ; Fraction of blood flow to the liver hepatic artery (Lin et al., 2020; Table 24)
QLpC = 0.28 ; Fraction of blood flow to the liver portal vein (from intestine, Lin et al., 2020; Table 24)
QKC = 0.10 ; Fraction of blood flow to the kidneys (Lin et al., 2020; Table 24)
QFC = 0.08 ; Fraction of blood flow to the fat (Lin et al., 2016 SS Table 2, search for calves)
QMC = 0.18 ; Fraction of blood flow to the muscle (Lin et al., 2016 SS Table 2, search for calves)
QLuC = 0.46 ; Fraction of blood flow to the lungs (Lin et al., 2020; Table 24);
QIC = 0.11 ; Fraction of blood flow to the GI tract (Lin et al., 2020; Table 24, search for intestines alone)
QRC = 0.21 ; Fraction of blood flow to the rest of the body (1-QLhC-QLpC-QKC-QFC-QMC-QGC)

; Mass Transer parameters (Chemical-specific parameters)
; partition coefficients
; Parent drug, enrofloxacin
PL = 4.3 ; Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK = 5.5 ; Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF = 0.53 ; Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM = 1.09 ; Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu = 4.3 ; Lung:plasma PC (Lin et al., 2016 SS Table 4)
PI = ? ; Intestine:plasma
PR = 1.5; Rest of body:plasma (Lin et al., 2016 SS Table 4)

; main metabolite, ciprofloxacin
PL1 = 4.3 ; Liver:plasma PC (Lin et al., 2016 SS Table 4)
PK1 = 5.5 ; Kidney:plasma PC (Lin et al., 2016 SS Table 4)
PF1 = 0.53 ; Fat:plasma PC (Lin et al., 2016 SS Table 4)
PM1 = 1.09 ; Muscle:plasma PC (Lin et al., 2016 SS Table 4)
PLu1 = 4.3 ; Lung:plasma PC (Lin et al., 2016 SS Table 4)
PI1 = ? ; Intestine:plasma
PR1 = 1.5 Rest of body:plasma  ; (Lin et al., 2016 SS Table 4)

KmC = 0.06 ; Hepatic metabolic rate [/(h*kg)] (Lin et al., 2016 SS Table 4)

Frac = 0.55 ; Fraction of ENR metabolized to CIP (unitless),  (Lin et al., 2016 SS Table 4)
PB = 0.46 ; ENR percentage of plasma protein binding (unitless)  (Lin et al., 2016 SS Table 4)
PB1 = 0.19 ; CIP percentage of plasma protein binding (unitless) (Lin et al., 2016 SS Table 4s)
Kfeces = NA ; Fecal elimination rate constant (/h) (Lin et al., 2016 SS Table 4)
KurineC = 0.15 ; ENR urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4)
Kurine1C = 1.99 ; ENR urinary elimination constant (L/h/kg) (Lin et al., 2016 SS Table 4)

; SC infusion rate constants
Timesc = 0.00 ; SC injection/infusion (h)

; Urinary elimination rate constant
KurineC = 0.15 ; Enro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)
KurineCm = 1.99, cipro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)

; Billiary elimination rate constant??
KfecesC = ? ; Enro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)
KfecesCm = ? ; cipro L/h/kg (Lin et al., 2016 SS Table 4, update by original references)

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

; ENRO in intestine compartment
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

; CIProfloxacin
; CIP in liver compartment
RL1 = QL*(CA-CVL)
d/dt(AL1) = RL1
init AL1 = 0
CL1 = AL1/VL
CVL1 = AL/(VL*PL)

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











