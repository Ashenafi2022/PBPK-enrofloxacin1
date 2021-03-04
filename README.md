# PBPK-enrofloxacin1

METHOD RK4
STARTTIME = 0
STOPTIME = 50
DT = 0.01
DTOUT = 0.01

; Physiological parameters
; Blood flow rates
QCC = 12.9 ; cardiac output (L/h/kg)
QLC = 0.297 ; Fraction of blood flow to the liver
QKC = 0.173 ; Fraction of blood flow to the kidneys
QFC = 0.097 ; Fraction of blood flow to the fat
QMC = 0.217 ; Fraction of blood flow to the muscle
QLuC = 0.000 ; Fraction of blood flow to the lungs

; Tissue volums
BW = 11.3 ; Body weight (kg)
VLC = 0.0329 ; Fractional liver tissue
VKC = 0.0055 ; Fractional kidney tissue
VFC = 0.15 ; Fractional fat tissue
VMC = 0.4565 ; Fractional muscle tissue
VbloodC = 0.082 ; Blood volume, fraction of BW
VLuC = 0.000 ; Fractional lung tissue

; Mass Transer parameters (Chemical-specific parameters)
; partition coefficients
PL = 1.89 ; Liver:plasma PC
PK = 4.75 ; Kidney:plasma PC
PF = 0.000 ; Fat:plasma PC
PLu = 0.000 ; Lung:plasma PC
PR = 0.000 ; Richly perfused tissues:plasma PC
PS = 0.000 ; Slowly perfused tissue:plasma PC

; SC infusion rate constants
Timesc = 0.00 ; SC injection/infusion (h)

; Urinary elimination rate constant
KurineC = 0.00 ; L/h/kg

; Parameters for exposure scenario
PDOSEsc = 7.5 ; (mg/kg), two doseses: 7.5 and 12.5

; Cardiac output and blood flows to tissues (L/h)
QC = QCC*BW ; Cardiac output
QL = QLC*QC ; Liver
QK = QKC*QC ; kidney
QF = QFC*QC ; Fat
QLu = QLuC#QC ; Lung
QM = QMC*QC ; Muscle
QR = 0.626*QC-QL-QK-QLu ; Richly perfused tissues
QS = 0.374*QC-QF-QM ; Slowly perfused tissues

; Tissue Volume (L)
VL = VLC*BW ; Liver
VK = VKC*BW ; Kidney
VF = VFC*BW ; Fat
VM = VMC*BW ; Muscle
VLu = VLuC*BW ; Lung
Vblood = VbloodC*BW ; Blood
VR = 0.142*BW-VL-VK-VLu ; Richly perfused tissues
VS = 0.776*BW-VF-VM ; Slowly perfused tissues

;Dosing
DOSEsc = PDOSEsc*BW ; (mg)

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

; ENRO in richly perfused compartment
RR = QR*(CA-CVR)
d/dt(AR) = RR
init AR = 0
CR = AR/VR
CVR = AR/(VR*PR)

; ENRO in slowly perfused compartment
RS = QS*(CA-CVS)
d/dt(AS) = RS
init AS = 0
CS = AS/VS
CSV = AS/(VS*PS)

; Urinary excretion of ENRO
Rurine = Kurine*CVK
d/dt(Aurine) Rurine
inti Aurine = 0

; Mass balance
Qbal = QC-QL-QK-QF-QM-QLu-QR-QS
Tmass = AA+AL+AK+AF+AM+ALu+AR+AS+Aurine
Ba; = Aiv-Tmass ; Mass balance











