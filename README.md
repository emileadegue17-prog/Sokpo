#include <stdio.h>

#define MAX 100

typedef struct {
    int numero;
    char nom[50];
    float dst;
    float examen;
    float moyenne_brute;
    float bonus;
    float moyenne_definitive;
} Etudiant;

/* ----------- saisie d’un étudiant ----------- */
Etudiant saisirEtudiant() {
    Etudiant e;

    printf("\nNumero : ");
    scanf("%d", &e.numero);

    printf("Nom : ");
    scanf("%s", e.nom);

    printf("Note DST : ");
    scanf("%f", &e.dst);

    printf("Note Examen : ");
    scanf("%f", &e.examen);

    return e;
}

/* ----------- calcul moyennes ----------- */
void calculMoyennes(Etudiant tab[], int n) {
    for(int i = 0; i < n; i++) {

        tab[i].moyenne_brute = 0.45 * tab[i].dst + 0.55 * tab[i].examen;

        if(tab[i].moyenne_brute < 10)
            tab[i].bonus = 1;
        else if(tab[i].moyenne_brute <= 15)
            tab[i].bonus = 0.5;
        else
            tab[i].bonus = 0;

        tab[i].moyenne_definitive = tab[i].moyenne_brute + tab[i].bonus;
    }
}

/* ----------- moyenne générale UE ----------- */
float moyenneUE(Etudiant tab[], int n) {
    float somme = 0;

    for(int i = 0; i < n; i++) {
        somme += tab[i].moyenne_definitive;
    }

    return somme / n;
}

/* ----------- affichage ----------- */
void afficherResultats(Etudiant tab[], int n, float mg) {
    printf("\n===== RESULTATS =====\n");

    for(int i = 0; i < n; i++) {
        printf("\nEtudiant %s", tab[i].nom);
        printf("\nMB = %.2f | MD = %.2f", tab[i].moyenne_brute, tab[i].moyenne_definitive);

        if(tab[i].moyenne_definitive >= mg)
            printf(" --> UE VALIDEE\n");
        else
            printf(" --> UE NON VALIDEE\n");
    }

    printf("\nMoyenne generale UE = %.2f\n", mg);
}

/* ----------- main ----------- */
int main() {
    Etudiant classe[MAX];
    int n;

    printf("Nombre d'etudiants : ");
    scanf("%d", &n);

    // saisie
    for(int i = 0; i < n; i++) {
        classe[i] = saisirEtudiant();
    }

    // calcul
    calculMoyennes(classe, n);

    // moyenne UE
    float mg = moyenneUE(classe, n);

    // affichage
    afficherResultats(classe, n, mg);

    return 0;
}
