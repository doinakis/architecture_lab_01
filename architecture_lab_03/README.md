 # architecture_lab_01
## **Τρίτο εργαστήριο αρχιτεκτονικής υπολογιστών (ΤΗΜΜΥ ΑΠΘ)** 
### Δοϊνάκης Μιχάλης 
### e-mail : doinakis@ece.auth.gr

### Εισαγωγή

Στο τρίτο και τελευταίο εργαστήριο μας ζητήθηκε να χρησιμοποιήσουμε ένα ακόμη εργαλείο για να παράξουμε εκτιμήσεις για τη κατανάλωση ισχύος, με βάση τα αποτελὲσματα μας από το δεύτερο εργαστήριο.Θα συγκρίνουμε και τα αποτελέσματα της συνάρτησης κόστους σε σχέση με το EDAP σε κάθε περίπτωση και θα δούμε αν αυτά τα δύο συμβαδίζουν. 

### Εξοικείωση με το McPAT
Παρατηρώντας την έξοδο του McPAT παρατηρούμε εγγραφές που έχουν να κάνουν με το dynamic power και το leakage.Το **Dynamic Power** είναι η ενέργεια που καταναλώνεται απο τη φόρτιση και την αποφόρτιση των χωρητικών φορτίων, όταν το σύτημα αλλάζει κατάσταση. Είναι ανάλογο της συνολικής χωρητικότητας , του ρολογιού , της παρεχόμενης τάσης , και της "ταλάντωσης" της τάσης καθώς αυτή αλλάζει.Το **Leakage** είναι η ενέργεια που καταναλώνεται εξαιτίας της διαρροής ρεύματος από το transistor. Έχουμε δύο είδη leakage:
* **Subthreshold Leakage**  
όπου ένα trsnsistor σε κατάσταση off επιτρέπει μικρή ποσότητα ρεύματος να περνάει μεταξύ της πηγής (source) και της εκροής(drain)  
* **Gate Leakage**  
που οφείλεται στη διαρροή του ρεύματος από την πύλη (gate) του transistor    

Για να κατανοήσουμε καλύτερα τις έννοιες ας φέρουμε ένα παράδειγμα.Αν τρέξουμε δύο διαφορετικά προγράμματα στον ίδιο επεξεργαστή αυτό που θα επηρεαστεί είναι το dynamic power, γιατί θα γίνει διαφορετική φόρτιση και αποφόρτιση των χωρητικών φορτίων.Γενικά, το dynamic power εξαρτάται και από τη δραστηριότητα στον επεξεργαστή και από τον όγκο των δεδομένων που μπορεί να επεξεργαστεί σε μία δεδομένη στιγμή, με το να θέτει τα transistor σε on και off state.Το leakage από την άλλη, είναι στατική κατανάλωση ενέργειας και δεν επηρεάζεται από την δραστηριότητα στον επεξεργαστή, αλλά μόνο από το ρεύμα που διαρρέει τα transtistor, όπως αναφέρθηκε και προηγουμένως.Για το dynamic power έχει σημασία και ο χρόνος εκτέλεσης του προγράμματος, ενώ το leakage δείχνει και το πόση ενέργεια θα κατανάλωνει το σύστημα αν βρίσκεται σε κατάσταση idle. 

#### Έστω ότι έχετε ένα σύστημα που τροφοδοτείται από μπαταρία συγκεκριμένης χωρητικότητας και έχετε την επιλογή να βάλετε στο σύστημα αυτό δύο διαφορετικούς επεξεργαστές. Έστω ότι ο ένας καταναλώνει 5 Watt και ο άλλος 40 Watt. Υπάρχει περίπτωση ο δεύτερος επεξεργαστής να δίνει στο σύστημα μεγαλύτερη διάρκεια μπαταρίας;  

Η σύντομη απάντηση είναι ναι υπάρχει περίπτωση να διαρκεί περισσότερο η μπαταρία, αλλά ας το πάρουμε από την αρχή.Αρχικά, αν υποθέσουμε ότι είχαμε αυτούς τους δύο επεξεργαστές σε κατάσταση idle χωρίς την εκτέλεση κάποιου task, τότε στο σύστημα με τα 40 Watt(Joule/s) θα τελείωνε πιο γρήγορα η μπαταρία εξαιτίας του περισσότερου leakage που θα υπήρχε λόγω των περισσότερων Watt.Από αυτή την οπτική το McPAT μας δίνει πληροφορία για τη διάρκεια της μπαταρίας. Ωστόσο, αυτό που θα πρέπει να λάβουμε υπόψιν είναι το energy efficiency, δηλαδή η ενέργεια που απαιτείται από τον κάθε επεξεργαστή για την εκτέλεση του ίδιου task.Αν για παράδειγμα έχουμε μία εφαρμογή η οποία τρέχει σε χρόνο 1 second και στους 2 επεξεργαστες τότε η ενέργεια που καταναλώνει ο πρώτος είναι 5 Joule ενώ ο δεύτερος 40 Joule.Για να φτάσουν να καταναλώνουν την ίδια ενέργεια θα πρέπει ο δεύτερος να είναι 8 φορές γρηγορότερος από τον πρώτο, δηλαδή ο πρώτος να χρειάζεται χρόνο 8 seconds και να καταναλώνει ενέργεια 40 Joule και ο δεύτερος χρόνο 1 second και ενέργεια 40 Joule.Για να διαρκέσει , λοιπόν, η μπαταρία στον δεύτερο περισσότερη ώρα, ή καλύτερα να πούμε για να καταναλώσει μικρότερο ποσό ενέργειας από την μπαταρία για το ίδιο task, θα πρέπει να είναι πάνω από 8 φορές γρηγορότερος σε σχέση με τον επεξεργαστή με τα 5 Watt, το οποίο είναι δυνατό να συμβεί.Επίσης, με το παράδειγμα που έφερα καταλαβαίνουμε ποια πληροφορία είναι αυτή που δεν μας παρέχεται από το McPAT και είναι αυτή του χρόνου.Το McPAT δεν μας δίνει πληροφορία για το πόσο χρόνο παίρνει το εκάστοτε πρόγραμμα για να τρέξει στον επεξεργαστή που εξομοιώνει, αλλά μας δίνει πληροφορία για το πόση ενέργεια καταναλώνει ανα δευτερόλεπτο ο επεξεργαστής.Ευτυχώς για εμάς όμως την πληροφορία αυτή μπορούμε να την αντλήσουμε από το gem5 😊(sim_seconds).  
#### Ας κάνουμε μια εντελώς αυθαίρετη υπόθεση και ας υποθέσουμε ότι o Xeon μπορεί να τρέξει την ίδια εφαρμογή με τον Α9, 40 φορές γρηγορότερα. Υποθέτοντας ότι δεν διακόπτεται η λειτουργία του συστήματος μετά την ολοκλήρωση εκτέλεσης της εφαρμογής (δλδ δεν απενεργοποιείται με κάποιο τρόπο το σύστημα), εξηγείστε χρησιμοποιώντας το McPAT γιατί o Xeon δεν μπορεί να είναι πιο energy efficient από τον A9 παρά τη διαφορά στην απόδοση.  
Από τα αποτελέσματα που προέκυψαν από την εκέλεση του McPAT παίρνουμε τις εξής πληροφορίες που φαίνονται στο παρακάτω πίνακα.  

|RESULTS         |ARM_A9      | XEON      | 
| :------------- |:----------:| :--------:| 
| Total Leakage  | 0.108687 W | 36.8319 W | 
| Runtime Dynamic| 2.96053 W  | 72.9199 W |  

Ας υποθέσουμε τώρα ότι για το ίδιο task ο Xeon τελειώνει σε 1 second, ενώ ο ARM_A9 αφού είναι 40 φορές πιο αργός τελειώνει σε 40 seconds. Αν κάνουμε τις πράξεις για να δούμε πόση ενέργεια χρειάστηκε ο καθένας για την εκτέλεση μόνο του task, βλέπουμε ότι ο Xeon χρειάστηκε 72.9199 Joule( Runtime Dynamic * time_needed ) και ο ARM_A9 χρειάστηκε 118.4212 Joule. Με αυτή τη πρώτη ματιά θεωρούμε ότι ο Xeon είναι πιο αποδοτικός.Όμως, επειδή το σύστημα μετά την εκτέλεση του task παραμένει ανοιχτό, πρέπει να λάβουμε υπόψιν και το leakage. Αν τρέχαμε και στους 2 ταυτόχρονα το ίδιο task ο Xeon θα τελείωνε το task γρηγορότερα αλλά θα είχε καταναλώσει συνολικά 72.9199 + 36.8319 * 39 = 1509,364 Joule (όπου 39 είναι τα δευτερόλεπτα που θα χρειαστεί για να τελειώσει ο ARM_A9) ενώ σε όλο αυτό το διάστημα ο ARM_A9 έχει καταναλώσει μόλις 118,4212 Joule και μετά την ολοκλήρωση του task καταναλώνει μόλις 0.108687 Joule ανά δευτερόλεπτο που περνάει.Το ίδιο ακριβώς ισχύει(με διαφορές στις πράξεις βέβαια) και με κάποιον χρόνο t εκτέλεσης ενός προγράμματος σε Xeon.Επομένως, ο Xeon δεν μπορεί να είναι πιο energy efficient από τον ΑRM_A9 παρά τη διαφορά στην απόδοση.  

### gem5 + McPAT αναζητώντας τη βελτιστοποίηση του γινομένου EDAP  

Για να καταφέρουμε να προσαρμόσουμε τις πληροφορίες που αντλήσαμε από το gem5 στο McPAT, χρησιμοποιήσαμε το pyhton script GEM5ToMCPAT.py, που βρίσκεται στο φάκελο scripts του McPAT.Το script παίρνε ως είσοδο του το αρχείο με τα stats από τη προσομοίωση του gem5 και το config.json και παράγει ένα αρχείο xml το οποίο έποιτα το χρησιμοποιούμε για να πάρουμε αποτελέσματα από το McPAT.Για να υπολογίσουμε το EDAP(ENERGY-DELAY-AREA PRODUCT) χρειαζόμαστε τις αντίστοιχες τιμές : 
* **Area**  
η πληροφορία βρίσκεται εύκολα στην έξοδο όταν τρέχουμε το McPAT 
* **Delay**  
το delay είναι ο χρόνος που απαιτήθηκε από το πρόγραμμά μας για να ολοκληρωθεί. Μπορούμε να αντλήσουμε τη πληροφορία αυτή από το αρχείο stats.txt του gem5 και είναι το sim_seconds 
* **Energy**  
Ο τρόπος που θα υπολογίσουμε την ενέργεια, αφού μας ζητείται να λάβουμε υπόψιν μόνο το Core και το L2 είναι ο εξής:

![energy_function](https://github.com/doinakis/architecture_lab_01/blob/master/architecture_lab_03/energy_function.png)

όπου CoreRuntime,L2Runtime,CoreSub,L2Sub,CoreGate,L2Gate είναι οι αντίστοιχες μεταβλητές που παίρνουμε από την εκτέλεση του McPAT και delay είναι το sim_seconds που προκύπτει από τα stats του gem5.  
Έχοντας, λοιπόν, όλα αυτά μπορούμε να υπολογίσουμε την συνάρτηση για το EDAP ως εξής:

![edap_function](https://github.com/doinakis/architecture_lab_01/blob/master/architecture_lab_03/edap_function.png)  

### Διαγράμματα σε σχέση με το Area και το Peak Power

Τα διαγράμματα τα οποία παρουσιάζονται παρακάτω αφορούν το specbzip, ωστόσο σε όλα τα benchmark η συμπεριφορά είναι παρόμοια, όπως φαίνεται και από τα αποτελέσματα που υπάρχουν στους υποφακέλους του repository. Ο λόγος που δεν παρουσιάζεται για κάθε ένα benchmark ξεχωριστά είναι για εξοικονόμηση χώρου και χρόνου.Να σημειωθεί ότι στα παρακάτω διαγράμματα παρουσιάζεται το συνολικό Area του επεξεργαστή και το Peak Power, σε σχέση με τις διάφορεσ τιμές που τρέξαμε στα προηγούμενα benchmark στην 2η εργασία.Επιπλέον, για τα benchmarks με icache = 16kΒ assoc = 8 και dcache = 16kB assoc 8 εμφανίζεται στα διαγράμματα η τιμή error διότι για τα συγκεκριμένα το mcpat δεν μπόρεσε να δώσει αποτελέσματα.  

![area_peak_power_cache_line_size](https://github.com/doinakis/architecture_lab_01/blob/master/architecture_lab_03/diagrams/area_peak_power_cache_line_size.png)  
Όπως ήταν αναμενόμενο, η αύξηση του cache line size οδήγησε τόσο σε αύξηση του Area όσο και σε αύξηση του Peak Power.  

![area_peak_power_L2](https://github.com/doinakis/architecture_lab_01/blob/master/architecture_lab_03/diagrams/area_peak_power_L2.png)  Η διαφορά στην L2, τόσο στο Area όσο και στο Peak Power, είναι αισθητή κυρίως με την αύξηση του μεγέθους και όχι τόσο με την αλλαγή στο associativity της L2 cache.  

![area_peak_power_dcache](https://github.com/doinakis/architecture_lab_01/blob/master/architecture_lab_03/diagrams/area_peak_power_dcache.png)  
![rea_peak_power_icache](https://github.com/doinakis/architecture_lab_01/blob/master/architecture_lab_03/diagrams/area_peak_power_icache.png)  
Για τα icache και dcache παρατηρείται και εδώ αύξηση του Area και του Peak Power με την αύξηση του assoc και του μεγέθους της αντίστοιχης cache.  
Σχεδόν η ίδια συμπεριφορά παρουσιάζεται για όλα τα benchmarks.Τα αρχεία από τα οποία αντληθηκαν οι παραπάνω πληροφορίες, αλλά και για τα άλλα benchmarks μπορούν να βρεθούν στο φάκελο mcpat_results.
### Βέλτιστο EDAP για κάθε benchmark(σύμφωνα με τον πίνακα με τα κόστη από το προηγούμενο εργαστήριο)

Στην προηγούμενη εργασία, είχε παρουσιαστεί ένας πίνακας ο οποίος παρουσίαζε τα ελάχιστα κόστη, σύμφωνα με τη συνάρτηση που είχα επινοήσει. Σε αυτό το σημείο θα υπολογίσουμε το edap για κάθε στοιχείο εκείνου του πίνακα ελάχιστων κόστων, και θα βρούμε την επιλογή που δίνει το ελάχιστο edap για κάθε benchmark και θα δούμε αν συμβαδίζει με εκείνη του ελάχιστου κόστους.     

| BENCHMARK          | L1 ISIZE | L1 DSIZE | ASSOC1 | ASSOC2 | L2 SIZE | ASSOC | CPI      | CACHE LIZE SIZE | COST    |EDAP|
| :---               |:---:     | :---:    | :---:  | :---:  | :---:   | :---: | :--:     | :--------------:| ----:   |----:|
| SPECBZIP(DEFAULT)  | 32kB     | 64kB     |  2     |   2    |  2MB    |   8   | 1,67965  |       64        | 1655,01 |0,14367|
| SPECBZIP(L2)       | 32kB     | 64kB     |  2     |   2    |  4MB    |   1   | 1,67965  |       64        | 1628,15 |0,199561|
| **SPECBZIP(CLS)**      | **32kB** | **16kB**     | **2**   |**1**    |  **2MB**    |  **8** | **1,852581** |  **32**| **1380,29** |**0,075905**|
| SPECBZIP(Dcache)   | 32kB     | 16kB     |  2     |   4    |  4MB    |   8   | 1,70806  |       256       | 3750,89 |4,244456|
| SPECBZIP(Icache)   | 16kB     | 64kB     |  1     |   2    |  4MB    |   8   | 1,65004  |       256       | 3870,99 |2,590563|
| SPECHMMER(DEFAULT) | 32kB     | 64kB     |  2     |   2    |  2MB    |   8   | 1,187917 |       64        | 1170,49 |0,151637|
| SPECHMMER(L2)      | 32kB     | 64kB     |  2     |   2    |  512kB  |   1   | 1,188545 |       64        | 1159,25 |0,048905|
| **SPECHMMER(CLS)**   | **32kB** |**64kB**  |**2**  |**2**  |**2MB**  | **8** | **1,196843** | **32**  | **892,04**  |**0,041723**|
| SPECHMMER(Dcache)  | 32kB     | 16kB     |  2     |   4    |  512kB  |   4   | 1,180538 |       256       | 2579,50 |1,009723|
| SPECHMMER(Icache)  | 16kB     | 64kB     |  2     |   2    |  512kB  |   4   | 1,180635 |       256       | 2760,74 |0,656655|
| SPECLIBM(DEFAULT)   | 32kB     | 64kB     |  2     |   2    |  2MB    |   8   | 3,493415 |       64       | 3442,17 |0,553045|
|**SPECLIBM(L2)**  | **32kB** | **64kB**     |  **2**     |   **2**    |  **1MB**|   **1**   | **3,495444** |**64** | **3412,71** |**0,441898**|
| SPECLIBM(CLS)      | 32kB     | 64kB     |  2     |   2    |  2MB    |   8   | 3,493415 |       64        | 3442,17 |0,553045|
| SPECLIBM(Dcache)   | 32kB     | 16kB     |  2     |   2    |  4MB    |   1   | 1,989325 |       256       | 4341,37 |3,666819|
| SPECLIBM(Icache)   | 16kB     | 64kB     |  1     |   2    |  4MB    |   1   | 1,989360 |       256       | 4653,11 |3,345082|
| SPECMCF(DEFAULT)   | 32kB     | 64kB     |  2     |   2    |  2MB    |   8   | 1,299095 |       64        | 1280,04 |0,085988|
| SPECMCF(L2)        | 32kB     | 64kB     |  2     |   2    |  2MB    |   2   | 1,299192 |       64        | 1272,34 |0,090142|
|**SPECMCF(CLS)** | **32kB** | **64kB** |  **2**     |   **2**|**2MB**  |**8**   | **1,260085** | **32** | **939,18**  |**0,045263**|
| SPECMCF(Dcache)    | 32kB     | 16kB     |  2     |   2    |  4MB    |   1   | 1,270126 |       32        | 638,02  |0,050258|
| SPECMCF(Icache)    | 16kB     | 64kB     |  4     |   2    |  4MB    |   1   | 1,189151 |       32        | 795,54  |0,063183|
| SPECSJENG(DEFAULT) | 32kB     | 64kB     |  2     |   2    |  2MB    |   8   | 10,270554|       64        | 10119,91|4,573403|
| **SPECSJENG(L2)** | **32kB**| **64kB** |  **2**|   **2**| **1MB** |  **1**   | **10,273011**|**64** | **10029,88**|**3,649789**|
| SPECSJENG(CLS)     | 32kB     | 64kB     |  2     |   2    |  2MB    |   8   | 6,799471 |       128       | 9963,49 |8,566108|
| SPECSJENG(Dcache)  | 32kB     | 16kB     |  2     |   2    |  4MB    |   2   | 5,172786 |       256       | 11293,91|23,360719|
| SPECSJENG(Icache)  | 16kB     | 64kB     |  1     |   2    |  4MB    |   2   | 5,173158 |       256       | 12105,18|21,738719|  
  
  Οι υλοποιήσεις με το καλύτερο edap φαίνονται με έντονο μαύρο χρώμα στον παραπάνω πίνακα.Παρατηρούμε ότι, για τα πρώτα τρία benchmarks, το ελάχιστο κόστος ταυτίζεται με το ελάχιστο edap. Ωστόσο στα δύο τελευταία benchamrks δεν συμβαίνει αυτό.Αυτό γίνεται διότι στη συνάρτηση κόστους δεν λαμβάνουμε υπόψιν την ενέργεια που δαπανάται από το κάθε σύστημα.Όμως αυτό δεν σημαίνει ότι και η επιλογή με το μικρότερο edap είναι η καλύτερη, αφού στο τελευταίο benchmark δίνει πολύ υψηλό CPI, το οποίο δεν θέλουμε. Γενικά, αν ανατρέξουμε τα αποτελέσματα του McPat μπορεί να βρούμε και χαμηλότερα edap για κάθε benchmark, ωστόσο στόχος της σύγκρισης αυτής ήταν να δούμε πώς επηρεάζεται το edap σε σύγκριση με το κόστος.  
  Γενικά, σε μία επιλογή των παραμέτρων για κάποιο πραγματικό επεξεργαστή θα πρέπει να ληφθούν υπόψιν τόσο κάποιου είδους συνάρτηση κόστους, όσο και το edap και συνήθως πάντα θα πρέπει να γίνουν ορισμένες υποχωρήσεις για να βρεθεί μία μέση λύση, ή μπορεί για συγκεκριμένου είδους προβλήματα η επιλογή να είναι και cost effective και να δίνει χαμηλό edap, αλλά να μη μπορεί να χρησιμοποιηθεί για γενικές εφαρμογές.  
    
 ### Αξιολόγηση εργαστηριακών ασκήσεων  
 #### Πρώτο Εργαστήριο 
 Η αλήθεια είναι ότι στην αρχή η πρώτη εργαστηριακή άσκηση φαινόταν αρκετά δύσκολη διότι δεν είχαμε καμία επαφη με τον gem5, αλλά ούτε και με παρόμοιο εργαλείο στο παρελθόν. Ωστόσο, μετά από αναζήτηση στη βιβλιογραφία και στο site του gem5 το πρώτο εργαστήριο ολοκληρώθηκε σχετικά εύκολα, αφού ήταν κυρίως βιβλιογραφικό και για εξοικείωση.
 #### Δεύτερο Εργαστήριο  
 Το δεύτερο εργαστήριο ήταν σαφώς πιο δύσκολο από το πρώτο, κυρίως, να τρέξουμε πάρα πολλές προσομοιώσεις, και εγώ όντας μόνος σε ομάδα , χρειάστηκα αρκετό χρόνο τόσο για την ολοκλήρωση των benchmarks όσο και για την σύνταξη της αναφοράς , τη παρουσίαση των διαγραμμάτων κτλ.Γενικά, από άποψη φόρτου εργασίας νομίζω το δεύτερο εργαστήριο είχε το περισσότερο.
 #### Τρίτο Εργαστήριο 
 Το τρίτο εργαστήριο, αφού ήταν συνέχεια του δεύτερου, δεν απαιτούσε χρόνο για να τρέξουμε προσομοιώσεις, αλλά ήταν σημαντική η συλλογή των αποτελεσμάτων των προσομοιώσεων που είχαμε από το προηγούμενο.Το τρίτο εργαστήριο ήταν αυτό που μου φάνηκε πιο ενδιαφέρον διότι χρησιμοποιούσε και τα δύο εργαλεία μαζί, και γενικά τα ερωτήματα ήταν αυτά που μου κίησαν το ενδιαφέρον, διότι ήταν συνδυαστικά.  
 Σαφώς πήρα γνώση από αυτό το εργαστήριο τόσο για το gem5 όσο και για το McPat, αλλά η αλήθεια είναι ότι σε ορισμένα σημεία θα το ήθελα πιο κατατοπιστικό, κυρίως σε ότι αφορούσε τον gem5 και το πώς ακριβως λειτουργεί γιατί η εξοικείωση στην αρχή ήταν δύσκολη. Η αρχιτεκτονική,γενικά, μου κίνησε το ενδιαφέρον και μελλοντικά θα ήθελα να ασχοληθώ με το αντικείμενο.  
 
 Για οποιαδήποτε επιδιόρθωση παρακαλώ επικοινωνήστε μαζί μου: doinakis@ece.auth.gr  
Βιβλιογραφία: [gem5.org](http://gem5.org/), [m5sim.org](http://www.m5sim.org/Main_Page),[https://www.spec.org/cpu2006/](https://www.spec.org/cpu2006/)
