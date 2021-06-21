# non-local-means---Gausan-filter---image-denoise

non local means - Gausan filter - image denoise - using cuda.

Η κεντρική ιδέα του προβλήματος είναι η αποθορυβοποίηση εικόνων με Γκαουσιανο θόρυβο που
δέχεται ως είσοδο, χρησιμοποιώντας τον αλγόριθμο non-local-mean. Το φίλτρο non-local-mean
υπολογίζει τον μέσο ορό όλων τον pixels, σταθμισμένο με την ομοιότητα τους από το αρχικό pixel
αναφοράς χρησιμοποιώντας την εξής σχέση:

denoiseImagef(x) = SUM{ w(x.y) * noisedImage(y) }

οπού ο πίνακας των σταθμισμένων βαρών w δείχνει την ομοιότητα της γειτονίας ενός pixel x από όλες
τις υπόλοιπες y γειτονίες. Οπού:

w(x.y) = 1/Z(x) * exp{ -(|| P(i)-P(j) ||^2)*G(a) / sigma^2 }

με P(i) και P(j) οι τετραγωνικές γειτονίες με κέντρα τα i και j pixel αντίστοιχα.

Ζ ένα παράγοντας κανονικοποιησης
και G(a) ένας πίνακας βαρών των γειτονιών(patches) ως προς το κεντρικό τους pixel.
# Σειριακή υλοποίηση
Ο κώδικας διαβάζει την εικόνα “name.txt” μέσω της συνάρτησης
readfile() και στην συνέχεια προσθέτει <<τυχαίο>> Γκαουσιανο θόρυβο
μέσω της συνάρτησης addnoise().

Έπειτα το κάθε pixel της εικόνας φιλτράρετε και επιστρέφετε
αποθορυβοποιημένο με την χρήση της συνάρτησης denoise(). Η
συνάρτηση denoise() καλείτε N*N φορές στην main() οπού κάθε φορά
δέχεται ως όρισμα ένα pixel της εικόνας με θόρυβο και σταδιακά μεσώ
τον βαρών w υπολογίζει και επιστρέφει το αντίστοιχο pixel που θα έχειη αποθορυβοποιημένη εικόνα. Η συνάρτηση “patch(...,int i, int j,...)”
επιστρέφει την τετραγωνική γειτονία με κέντρο το pixel(i,j) της εικόνας.
Επιπλέον στο τέλος της main() υπολογίζετε και ένας πίνακας
differences που περιέχει της διάφορες της εικόνας με θόρυβο και της
αποθορυβοποιημένης εικόνας. Τέλος ο πίνακας differences γράφετε σε
ένα .txt αρχείο.

# Παράλληλη υλοποίηση με cuda
Στην παράλληλη υλοποίηση η πολυπλοκότητα από N^4 πέφτει σε Ν^2
μέσω των συναρτήσεων __global__ void CudaPatch() και __global__
void nonLocalMeans() οι οποίες αντικαθιστούν τις for() στην main που
καλούσαν τις συναρτήσεις patch() και denoise() οι οποίες έγιναν
__devise__.
