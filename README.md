http://gem5.org/Main_Page# architecture_lab_01
## **Πρώτο εργαστήριο αρχιτεκτονικής υπολογιστών (ΤΗΜΜΥ ΑΠΘ)** 

### Εισαγωγή

Στο πρώτο εργαστήριο μας ζητήθηκε να χρησιμοποιήσουμε τον gem5 για να εκτελέσουμε ένα απλό πρόγραμμα τύπου _"Hello World"_ προκειμένου να εξοικειωθούμε με τη χρήση του προσομοιωτή και να μάθουμε να αλλάζουμε, αλλά και να διαβάζουμε, τις διάφορες παραμέτρους από έναν επεξεργαστή.Ο gem5 έχει τη δυνατότητα να προσομοιώση και _Full System_ δηλαδή ένα πλήρες σύστημα και να εκτελέσει ενα πλήρες λειτουργικό σύστημα,ή να κάνει ενα _System Call Emulation_ δηλαδή να προσομοιώσει μόνο τον επεξεργαστή και το υποσύστημα της μνήμης.    

### Βασικές παράμετροι (με τη χρήση της starter_se.py)

  Ανοίγοντας το αρχείο _starter_se.py_ παρατηρούμε τα τρία διαφορετικά είδη cpu που μπορούμε να χρησιμοποίσουμε με τη συγκεκριμένη βιβλιοθήκη.Στη περίπτωση μας,εφόσον έχουμε τρέξει την εντολή _$ ./build/ARM/gem5.opt -d hello_result configs/example/arm/starter_se.py --cpu="minor" "tests/test-progs/hello/bin/arm/linux/hello"_ , έχουμε χρησιμοποιήσει το _flag --cpu="minor"_ άρα ήδη μπορούμε να βγαλουμε συμπεράσματα για τα είδη των _cache_ που χρησιμοποιεί ο συγκεκριμένος επεξεργαστής: 
* level 1 instruction cache 
* level 1 data cache 
* Walk Cache και 
* level 2 cache
* chache line size = 64kB.  
  Παράλληλα, με τη χρήση της συγκεκριμένης βιβλιοθήκης, τίθεται το **_Source Clock Domains_** στα 1 GHz και το **_Voltage Domain_** στα 3.3V.Το πρώτο διατηρεί την περίοδο ρολογιού και παρέχει μεθόδους για τη ρύθμιση / λήψη του ρολογιού και των παραμέτρων διαμόρφωσης που θα διαχειριστεί ο _handler_ όπως π.χ τιμές συχνότητας σε διάφορα επίπεδα απόδοσης. To _Voltage Domain_ χρησιμοποιείται για να ομαδοποιήσει τα _clock domains_ να λειτουργόυν κάτω από το ίδιο _voltage_, και παρέχει μεθόδους τόσο για τη ρύθμιση όσο και για τη λήψη του.  
Όπως παρατηρούμε και από την εντολή που εκτελέσαμε, η μόνη παράμετρος που θέσαμε είναι ο τύπος του επεξεργαστή που θα χρησιμοποιήσουμε (με το _flag --cpu="minor"_).Τι συμβαινει όμως με τις υπόλοιπες παραμέτρους του επεξεργαστή που δεν θέσαμε;Αν δεν βάλουμε συγκεκριμένες τιμές για τη συχνότητα, τον αριθμό των βασικών μονάδων, του τύπου της μνήμης κτλ η συγκεκριμένη βιβλιοθήκη δίνει κάποιες _default_ τιμές για οτιδήποτε δεν έχουμε καθορίσει.Παρέχει όμως τη δυνατότητα αλλαγής των συγκεκριμένων τιμών με τα κατάλληλα _flags_ ακριβώς όπως χρησιμοποιήσαμε και για τον τύπο του επεξεργαστή.Οι _default_ τιμές είναι οι εξής(σε παρένθεση εμφανίζονται τα _flags_ με τα οποία μπορούμε να αλλάξουμε τις τιμές) :
* Cpu frequency = 4GHz (_--cpu-freq_)
* Number of cores = 1 (_--num-cores_)
* Memory type = DDR3_1600_8x8 (_--mem-type_)
* Memory channels = 2 (_--mem-channels_)
* Memory ranks = None (_--mem-ranks_)
* Memory size = 2 GB (_--mem-size_)

### Επαλήθευση των παραμέτρων (με βάση τα αρχεία _config.ini config.json_)

#### Αρχείο _config.ini_
  Ανοίγοντας τώρα το αρχείο _config.ini_ το πρώτο πράγμα που παρατηρούμε είναι το **_[root]_** και κοιτώντας την παράμετρο **_full_system=false_** καταλαβαίνουμε ότι όντως πρόκειται για μία εξομοίωση όχι ενός ολοκληρωμένου συστήματος αλλά μόνο προσομοίωση του επεξεργαστή και των υποσυστημάτων μνήμης. Συνεχίζοντας την ανάγνωση βλέπουμε ένα ακόμα "αντικείμενο" με το όνομα **_[system]_** το οποίο έχει ως "παιδία" το **_clock domain_**,το **_voltage domain_** τα οποία αναφέρθηκαν παραπάνω το **_cpu_cluster_** το **_memory bus_** καθώς και το γεγονός ότι έχουμε .Επιπλέον, παρατηρούμε ότι το **_system_** είναι παιδί του **_root_**. Το _system_ μας δίνει πληροφορίες τόσο για το **_cache line size_ = 64 kB**, όπως είχαμε δει και στο **_starter_se.py_** αρχείο, αλλά μας δίνει και άλλες πληροφορίες σχετικά με το αν υποστηρίζεται _multi threading_, που στη περιπτωσή μας δεν υποστηρίζεται αφού έχουμε μονοπύρηνο επεξεργαστή, με τo ότι έχουμε _dual channel memory_ (**_memories=system.mem_ctrls0 system.mem_ctrls1_**) και το μέγεθος της _ram_ **_mem_ranges=0:2147483647_**,που αντισοιεί σε **2GB** _ram_.Όσον αφορά το _Source Clock Domains_ βρίσκεται στην ενότητα **_[system.clk_domain]_** και είναι ίσο με **_clock_=1000** ticks τα οποία αντιστοιχούν σε **1000000000000/1000=1GHz**, όσο αναμέναμε δηλαδή.Λίγο πιο κάτω στην ενότητα **_[system.cpu_cluster.clk_domain]_** βλέπουμε τη συχνότητα του επεξεργαστή **_clock_=250** ticks δηλαδή **1000000000/250=4GHz**, και στη συνέχεια στην **_[system.cpu_cluster.cpus]_** βλέπουμε πλέον πληροφορίες που έχουν να κάνουν με τον τύπο του επεξεργαστή (**_type=MinorCPU_**) αλλά και τον αριθμό των threads (**_numThreads=1_**).Επιπλέον, από τα "παιδιά" της ενότητας αυτής παίρνουμε πληροφορία σχετικά με τα είδη της _cache_ θα έχουμε _data cache_(**dcache**), _instruction cache_(**icache**) και _walker cache_(**dtb_walker_cache & itb_walker_cache**), αλλά και _level 2 cache_ που φαίνεται παρακάτω στην ενότητα (**_[system.cpu_cluster.l2]_**) .Κάθε είδος _cache_ έχει αργότερα τη δική του ενότητα που περιγράφει αναλυτικά τις προδιαγραφές τους π.χ αν υπάρχει _associativity_ το μέγεθος το _indexing_ και ότι αφορά το κάθε είδους _cache_.Το _voltage domain_ φαίνεται στην ενότητα **_[system.voltage_domain]**_ και έχει τιμή **_voltage=3.3_**.
  
  #### Αρχείο _config.json_
  Ουσιαστικά και το αρχείο _congif.json_ περιέχει ακριβώς τις ίδιες πληροφορίες με το αρχείο -config.ini_ απλά σε διαφορετική μορφή.Η αλήθεια είναι πως το το _config.ini_ είναι πιο ευανάγνωστο σε σχέση με το άλλο. 
  #### Επιπλέον αρχεία 
  Παράλληλα, με τα παραπάνω αρχεία ο gem5 δίνει ακόμα και μια γραφική αναπαράσταση του επεξεργαστή που έτρεξε το πρόγραμμα μας,στο οποίο φαίνονται και τα caches και ο τύπος της ram που χρησιμοποιήθηκε,καθώς και ένα αρχείο με στατιστικά για το πρόγραμμα που εκτελέστηκε,το οποίο θα το δούμε και παρακάτω.
  Γενικότερα, τα αρχεία που βγαίνουν μετά την ολοκλήρωση της προσομοίωσης απο το gem5 είναι ιδιαίτερα σημαντικά.Τα _config_ αρχεία μας επιτρέπουν να βεβαιωθούμε ότι η προσομοίωση έτρεξε ακριβώς με τις προδιαγραφές τις οποίες είχαμε θέσει πριν ξεκινήσουμε την προσομοίωση και να βεβαιωθούμε ότι δεν έχει γίνει κάποιο λάθος.Το αρχείο με τα στατιστικά είναι ακριβώς ο λόγος για τον οποίο τρέχουμε το πρόγραμμα μας στον _gem5_, για να δούμε τη συμπεριφορά του επεξεργαστή ή ενός ολόκληρου λειτουργικού όταν τρέχει σε αυτό το προγραμμά μας, τον χρόνο τα _cache misses_ και πολλές άλλες παραμέτρους που παρουσιάζονται μέσα σε αυτό.Όσο για τη γραφική αναπαράσταση που δίνει, μας δίνει μια συνολική εικόνα για το σύστημα για το οποίο τρέξαμε την προσομοίωση.
  
  ### _Gem5 in-order CPUs_ (HPI)
  #### _MinorCPU_
  Ένα από τα μοντέλα _in-order CPUs_ είναι και ο _MinorCPU_ που χρησιμοποιήσαμε παραπάνω για την εκτέλεση του _hello world_ προγραμματός.Ο _Minor_ είναι ένας _in order CPU_ ο οποίος έχει σταθερό _pipeline_, αλλά δυναμικές δομές δεδομένων και εκτελεστική συμπεριφορά.Χρησιμοποιείται κυρίως για αυστηρά _in order_ συμπεριφορές και επιτρέπει την απειόνιση μιας εντολής στο _pipeline_ μέσω του _MinorTrace/minorview.py format/tool_. 
O _MinorCPU_ αποτελεί ένα λεπτομερές μοντέλο επεξεργαστή για να προσομοιώσει ρεαλιστικά συσήματα.Σκοπός του είναι να δοθεί ένα πλαίσιο για την αρχιτεκτονική προσέγγιση του μοντέλου με έναν συγκεκριμένο επεξεργαστή με παρόμοιες ιδιότητες.Το μοντέλο αυτό δεν υποστηρίζει ακόμα _multithreading_ αλλά υπάρχουν _THREAD comments_ σε συγκεκριμένα σημεία όπου τα εκάστοτε δεδομένα χρειάζεται να προσαρμοστούν για να υποστηρίξουν _multithreading_.Χρησιμοποιεί _pipeline_ τεσσάρων σταδίων:
  * _fetch1_
  * _fetch2_
  * _decode_
  * και _execute_  
Για περισσότερες πληροφορίες αναλυτικά με το _pipeline_ το πώς γίνονται _issue_ οι εντολές πώς γίνεται το commit πώς γίνεται fetch κτλ το _site_ του _gem5_ έχει όλες τις πληροφορίες [**εδώ**](http://pages.cs.wisc.edu/~swilson/gem5-docs/minor.html).

 #### _SimpleCPU_
 Το μοντέλο _SimpleCPU_ είναι ένα μοντέλο ιδανικό όταν δεν είναι απαραίτητο ένα πλήρες και λεπτομερές μοντέλο.Αυτό μπορεί να περιλαμβάνει _warm up periods_ ή απλά μια επιβαιβέωση ότι το πρόγραμμα δουλεύει.Πλέον έχει χωριστεί σε τρεις κατηγορίες:
 * _BaseSimpleCPU_
 * _AtomicSimpleCPU_
 * _TimingSimpleCPU_  
 To _BaseSimpleCPU_ δεν μπορεί να λειτουργήσει αυτόνομο, αλλά πρέπει να χρησιμοποιηθει μαζί του και ένα από τα άλλα 2 μοντέλα. 
 Ο _AtomicSimpleCpu_ είναι η έκδοση του _SimpleCPU_ που χρησιμοποιεί _atomic memory accesses_, δηλαδή πιο γρήγορες "επισκέψεις"  στη μνήμη από ότι ένα λεπτομερές _access_.Αυτού του είδους τα _accesses_ χρησιμοποιούνται για να μεταβαίνουν πιο γρήγορα σε διαφορετικές καταστάσεις τους συστήματος (_fast forwarding_) και για να "προθερμαίνουν" τις _caches_ και επιστρέφουν έναν προσεγγιστικό χρόνο για να ολοκληρώσουν το ζητούμενο χωρίς καμία καθυστέρηση.Όταν καλείται ένα _atomic access_ η απάντηση σε αυτό παρέχεται όταν ολοκληρωθεί η συνάρτηση.Η _AtomicSimpleCpu_ χρησιμοποιεί τις προσεγγίσεις των καθυστερήσεων απο τα _atomic accesses_ για να υπολογίσει τον συνολικό _access time_ στη _cache_.Χρησιμοποιείται μαζί με τον _BaseSimpleCPU_ και παρέχει τις συναρτήσεις για τα _read_ και _write_ στη μνήμη και στο _tick_ που καθορίζει τι γίνεται σε κάθε κύκλο.Επιπλέον, ορίζει την "δίοδο" που προσαρτεί τη μνήμη και συνδέει τον επεξεργαστή με την _cache_.[_AtomicSimpleCPU_](http://www.m5sim.org/wiki/images/e/e5/AtomicSimpleCPU.jpg)  
 Ο _TimingSimpleCPU_ είναι η έκδοση που χρησιμοποιεί _timing memory accesses_, δηλαδή τις πιο λεπτομερείς "επισκέψεις" στη μνήμη.Είναι πρακτικά η πιο ρεαλιστική περίπτωση και περιλαμβάνει και τις καθυστερήσεις ουράς αλλα και αν δεν είναι διαθέσιμο κάποιο κομμάτι _hardware_. Μετά που ζητηθεί ένα _timing request_ κάποια στιγμή η συσκευή που το έστειλε θα λάβει μία απάντηση σε αυτό που ζήτησε ή ένα _NACK_  αν αυτό που ζητήθηκε δν μπόρεσε να ολοκληρωθεί.(Γενικά _timing_ και _atomic accesses_  δεν μπορούν να συνυπάρχουν σε ένα σύστημα.Ο _TimingSimpleCPU_, λοιπόν,κάνει _stall_ σε κάθε _access_ στη _cache_ και περιμένει την μνήμη του συστήματος να "απαντήσει" για να συνεχίσει.Ὀπως και η _AtomicSimpleCPU_ xρησιμοποιείται μαζί με τον _BaseSimpleCPU_ και παρέχει τις συναρτήσεις για τα _read_ και _write_ στη μνήμη και στο _tick_ που καθορίζει τι γίνεται σε κάθε κύκλο.Επιπλέον, ορίζει την "δίοδο" που προσαρτεί τη μνήμη και συνδέει τον επεξεργαστή με την _cache_.[_TimingSimpleCPU_](http://www.m5sim.org/wiki/images/f/f8/TimingSimpleCPU.jpg)  
 Και ο _AtomicSimpleCpu_ αλλά και ο _TimingSimpleCPU_ αποτελούν μοντέλα _fast-to-run_, καθώς απλοποιούν κάποιες πτυχές όπως το _pipeline_ που σημαίνει ότι μόνο μια εντολή εκτελείται κάθε στιγμή.Συνήθως χρησιμοποιούνται για να φτάσουμε "γρήγορα" σε κάποιο σημείο ενδιαφέροντος.
 #### _High performance in-order CPU_ 
 Το μοντέλο _HPI_ έχει ακριβώς την ίδιο _pipeline_ τεσσάρων σταδίων που έχει και ο _MinorCPU_. Το _timing_ μοντέλο που χρησιμοποιεί είναι αντιπροσωπευτικό για έναν μοντέρνο _in-order Armv8-A_.Πληροφορίες πιο αναλυτικές σχετικά με τα _caches_,το _pipeline_, τη διαχείριση της μνήμης και για πληροφορίες και για τα προηγούμενα μοντέλα μπορούν να βρεθούν [εδώ(αρχείο pdf)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&ved=2ahUKEwjA0oi7kvblAhVFI1AKHTz-AC4QFjACegQIBBAC&url=https%3A%2F%2Fraw.githubusercontent.com%2Farm-university%2Farm-gem5-rsk%2Fmaster%2Fgem5_rsk.pdf&usg=AOvVaw1pjkMJ--WpHuBWZmcFiV3q).  
 ### Δοκιμή του _gem5_ με ένα απλό πρόγραμμα
 Κληθήκαμε να γράψουμε ένα απλό πρόγραμμα σε γλώσσα _C_ για να πειραματιστούμε με τον _simulator_.Το πρόγραμμα,λοιπόν, που βρίσκεται μέσα στο αρχείο _computer_architecture_program.c_ αποθηκεύει 10000 _integer_ τυχαίους αριθμούς σε έναν πίνακα και στη συνέχεια τους εκτυπώνει σε αύξουσα σειρά με τη χρήση της συνάρτησης _qsort_ που υπάρχει ήδη υλοποιημένη στις βιβλιοθήκες της _C_.Για να καταφέρουμε να εκτελέσουμε το πρόγραμμα μας στον _gem5_ πρέπει να το κάνουμε _compile_ έτσι ώστε να υποστηρίζει _ARM_
αρχιτεκτονική.Αυτό το κάνουμε με την εντολή : arm-linux-gnueabihf-gcc --static computer_architecture_program.c -o myprog_arm η οποία θα δημιουργήσει ένα _executable_ αρχείο με το όνομα _myprog_arm_ το οποίο τώρα είναι σε θέση να το τρέξει ο _gem5_.Θα εκτελέσουμε την προσομοίωση για δύο είδη επεξεργαστών:
* τον _MinorCPU_
* τον _TimingSimpleCPU_  
τους οποίους αναλύσαμε παραπάνω.Όλα τα αρχεία της προσομοίωσης θα βρίσκονται στους φακέλους _MinorCPU_ και _TimingSimpleCPU_.Ο τρόπος με τον οποίο θα συγκρίνουμε τις δύο αυτές προσομοιώσεις είναι απο τα αρχεία _stats.txt_ που θα προκύψουν για κάθε ένα από αυτά.Γενικά, ένα αρχείο _stats.txt περιέχει πληροφορίες για κάθε ένα από τα _SimObject_(είναι κάθε παράμετρος που θέτουμε για τον επεξεργαστή ή τις _ram_ κτλ.) που θέτει.Στο τέλος κάθε προσομοίωσης αυτά τα στατιστικά αποθηκεύονται σε αυτό το αρχείο.Τα χαρακτηριστικά που θα χρησιμοποιήσουμε για τη σύφκριση έχουν να κάνουν με το πόσος χρόνος συστήματος απαιτήθηκε για να ολοκληρωθεί το πρόγραμμα μέσα στη προσομοίωση.  
Την πρώτη φορά που τρέχουμε την προσομοίωση και για τα δύο είδη _cpu_ το μόνο που καθορίζουμε είναι ο τύπος που θα χρησιμοποιηθεί σε κάθε περίπτωση ( τρεχουμε την εντολή με _flag --cpu-type="MinorCPU"/"TimingsimpleCPU" και --caches -c) διαρηρώντας όλες τις άλλες παραμέτρους σε _default_ τιμές και ίδιες και για τους δύο επεξεργαστές.Σε αυτό το σημείο να σημειωθεί ότι λόγω της τυχαιότητας που υπάρχει δεν θα τρέχει πάντα ακριβώς με τα ίδια δεδομένα το πρόγραμμα.  
Κοιτώντας τώρα μέσα στο αρχείο _stats.txt_ του φακέλου _MinorCPU_ βλέπουμε τις εξής παραμέτρους:
* **sim_ticks   7040836000**  Number of ticks simulated
* **sim_insts     11987731**  Number of instructions simulated
* **sim_seconds   0.007041**  Number of seconds simulated 
* **host_seconds     27.95**  Real time elapsed on the host  
 ενώ στο αρχείο _stats.txt_ του φακέλου _TimingSimpleCPU_ :
* **sim_ticks  18320148000**  Number of ticks simulated
* **sim_insts     11946875**  Number of instructions simulated
* **sim_seconds   0.018320**  Number of seconds simulated  
* **host_seconds      8.78**  Real time elapsed on the host  
Έχοντας αναλύση και τα δύο μοντέλα παραπάνω, όντως παρατηρούμε ότι ο χρόνος προσομοίωσης είναι μικρότερος στο _fast-to-run_ μοντέλο (_SimpleTimingCpu_), ενώ στον _MinorCpu_ είναι σχεδόν ο τριπλάσιος!Ο αριθμός των εντολών που εκτελέστηκαν κάθε φορά εξαρτάται και από το πως έχουν μπει οι τυχαίει αριθμοί στον πίνακα,όμως εδώ βλέπουμε ότι οι τιμές είναι πολύ κοντά.Ο χρόνος στον οποίο εκτελέστηκε το πρόγραμμά μας στη προσομοίωση βλέπουμε ότι είναι μικρότερος στο _MinorCPU_,αφού σε αυτόν έχουμε _pipeline_ σε αντίθεση με το άλλο μοντέλο όπου μόνο μία εντολή εκτελείται κάθε στιγμή.Άρα σε αυτή την περίπτωση τα μοντέλα παράγουν διαφορετικά αποτελέσματα γεγονός το οποίο αναμέναμε.  
#### Αλλαγή της συχνότητας του επεξεργαστή και της τεχνολογίας της μνήμης
Οι _default_ τιμές που είχαν δοθεί ήτα **_2GHz_** και **_DDR3_1600_8x8_**,οι οποίες αλλάχτηκαν σε **_5GHz_** και **_DDR4_2400_8x8_** αντοίστοιχα, με τις αλλαγές να φαίνοντα στα αρχεία **_MinorCPU2 MinorCPU3_** και **_TimingSimpleCPU2 TimingSimpleCPU3_**.  
Ας αναλύσουμε αρχικά τα δεδομένα αυτά για το κάθε επεξεργαστή ξεχωριστά.Για τον **_MinorCPU_** παρατηρούμε ότι η απόδοση αυξάνεται δραματικά για τα **_5GHz_** καθώς μειώνει το χρόνο σε **0.002822s** από **0.007041s**, ενώ αντίθετα η αλλαγή στη τεχνολογία της μνήμης δεν έφερε καθόλου βελτίωση με τον χρόνο να παραμένει στα **0.007035s**.Για τον **_TimingSimpleCPU_** 
και εδώ ο χρόνος μειώθηκε ,και σχεδόν ισοφάρισε το χρόνο του πρώτου _MinorCPU_, στα **_0.007432s_** από τα **_0.018320_**, ενώ η αλλαγή στην τεχνολογία της μνήμης έδωσε χρόνο **_0,018325_**.  
Ο λόγος που με η αλλαγή στη τεχνολογία της μνήμης _ram_ δεν επέφερε καθόλου βελτίωση στο χρόνο εκτέλεσης του προγράμματος είναι λογικά γιατί το πρόγραμμα μας δεν απαιτεί πολλές επισκέψεις στη μνήμη, ενώ αντίθετα η συχνότητα του επεξεργαστή είναι καθοριστική για την εκτέλεση του προγράμματος σε αυτή τη περίπτωση.  
Τέλος, γίνεται αντιλιπτό πως το _pipeline_του _MinorCPU_ του δίνει πλεονέκτημα ακόμα και όταν αυξηθεί το ρολόι του _TimingSimpleCPU_, παραμένει περίπου δύο φορές γρηγορότερος. 

#### Δοϊνάκης Μιχάλης 
Για οποιαδήποτε επιδιόρθωση παρακαλώ επικοινωνίστε μαζί μου: doinakis@ece.auth.gr
Βιβλιογραφία: [gem5.org](http://gem5.org/), [Arm Research Starting Kit(PDF)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&ved=2ahUKEwjA0oi7kvblAhVFI1AKHTz-AC4QFjACegQIBBAC&url=https%3A%2F%2Fraw.githubusercontent.com%2Farm-university%2Farm-gem5-rsk%2Fmaster%2Fgem5_rsk.pdf&usg=AOvVaw1pjkMJ--WpHuBWZmcFiV3q)
