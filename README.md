# PBPK-enrofloxacin1
## Barekely Madona codes for seven caompartments BPPK model for enrofloxacin in calves
### 7 compartments: liver, kidney, lungs, muscle, fat, plasma and the rest of the body
#### Two submodels: parent drug/enrofloxacin (seven compartments) and metabolite/ciprofloxacin (muscle and fat excluded)
METHOD RK4
STARTTIME = 0
STOPTIME = 50 # Data of plasma and fecal samples collected upto 48 hrs is available for model validation
DT = 0.001
DTOUT = 0.1

; Physiological parameters
; Blood flow rates
QCC = 5.67 ; cardiac output (L/h/kg) (Lin et al., 2016 SS Table 2, update by original references)
QLC = 0.35 ; Fraction of blood flow to the liver (Lin et al., 2016 SS Table 2, update by original references)
QKC = 0.09 ; Fraction of blood flow to the kidneys (Lin et al., 2016 SS Table 2, update by original references)
QFC = 0.08 ; Fraction of blood flow to the fat (Lin et al., 2016 SS Table 2, update by original references)
QMC = 0.18 ; Fraction of blood flow to the muscle (Lin et al., 2016 SS Table 2, update by original references)
QRC = 0.30 ; Fraction of blood flow to the rest of the body (1-QLC-QKC-QFC-QMC)
QLuC = 1.00 ; Fraction of blood flow to the lungs (Lin et al., 2020)

; Tissue volumes
BW = 118 ; Body weight (kg) (median of 35 calves in FQ AMR cattle study)
VAC = 0.0104 ; Fractional arterial blood (Lin et al., 2016 SS Table 2, update by original references); plasma?
VVC = 0.0296 ; Fractional venous blood (Lin et al., 2016 SS Table 2, update by original references)
VLC = 0.013 ; Fractional liver tissue (0.0204 in Lin et al., 2020 Table7; Lin et al., 2016 SS Table 2, update by original references)
VKC = 0.0035 ; Fractional kidney tissue (0.0037 in Lin at al., 2020 Table 7; Lin et al., 2016 SS Table 2, update by original references)
VFC = 0.15 ; Fractional fat tissue (0.1544 Lin et al., 2020 Table 7; Lin et al., 2016 SS Table 2, update by original references)
VMC = 0.27 ; Fractional muscle tissue (0.3632 in Lin et al., 2020 table 7; Lin et al., 2016 SS Table 2, update by original references)
;VbloodC = 0.082 ; Blood volume, fraction of BW #??? is VbloodC different from VAC/VVC? IS it plasma? (0.0412 in Lin et al., 2020 Table 7)
VLuC = 0.0123 ; Fractional lung tissue in calves (0.0123 in Lin 2020 Table 7; 0.008 in Lin et al., 2016 SS Table 2, update by original references)
VRC = 0.5155 ; Fractional rest of body (Lin et al., 2016 SS Table 2, OR 1-VAC-VVC-VLC-VKC-VFC-VMC-VLuC)

; Mass Transer parameters (Chemical-specific parameters)
; partition coefficients
; Parent drug, enrofloxacin
PL = 4.3 ; Liver:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PK = 5.5 ; Kidney:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PF = 0.53 ; Fat:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PM = 0.1.09 ; Muscle:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PLu = 4.3 ; Lung:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PR = 1.5; Rest of body:plasma (Lin et al., 2016 SS Table 4, update by original references)

; main metabolite, ciprofloxacin
PLm = 4.3 ; Liver:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PKm = 5.5 ; Kidney:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PFm = 0.53 ; Fat:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PMm = 0.1.09 ; Muscle:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
PLum = 4.3 ; Lung:plasma PC (Lin et al., 2016 SS Table 4, update by original references)
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

; Parameters for exposure scenario
PDOSEsc = 7.5 ; (mg/kg), two doseses: I want to model two dose regimns 7.5 and 12.5, how? (PDOSEscL/low dose=7.5 and PDOSEscH/High dose =12.5?)

; Cardiac output and blood flows to tissues (L/h)
QC = QCC*BW ; Cardiac output
QL = QLC*QC ; Liver
QK = QKC*QC ; kidney
QF = QFC*QC ; Fat
QLu = QLuC#QC ; Lung
QM = QMC*QC ; Muscle
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
VR = VRC*BW ; Rest of the body
; VR = 0.142*BW-VL-VK-VLu ; Richly perfused tissues
; VS = 0.776*BW-VF-VM ; Slowly perfused tissues

;Dosing
DOSEsc = PDOSEsc*BW ; (mg) (how to embrace low and high doses here?)

; Enro sc injection to the venous
SCR = DOSEsc/Timesc
RSC = SCR*(1.-step(1, Timesc)
d/dt(Asc) = RSC
init Asc = 0

; Urinary elimination rate constant
Kurine = KurineC*BW

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

; CIPRO in rest of body compartment


; Urinary excretion of ENRO
Rurine = Kurine*CVK
d/dt(Aurine) Rurine
inti Aurine = 0

; Fecal excretion of CIPRO

; Urinary excretion of CIPRO

; Fecal excretion of CIPRO

; Mass balance
Qbal = QC-QL-QK-QF-QM-QLu-QR-QS
Tmass = AA+AL+AK+AF+AM+ALu+AR+AS+Aurine
Ba; = Aiv-Tmass ; Mass balance











