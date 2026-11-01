# Mon Agenda & Alarmes — Android

Projet Android natif en Kotlin + Jetpack Compose.

## Fonctions incluses
- Agenda local : création et liste des événements.
- Pour chaque événement : choix entre notification Android et création d'une alarme via l'application Horloge.
- Délai réglable : à l'heure, 5, 15, 30, 60 minutes, la veille ou minutes personnalisées.
- Les notifications programmées sont restaurées après redémarrage du téléphone.

## Construire l'APK
1. Installer Android Studio (version récente) et le JDK 17.
2. Ouvrir le dossier `MonAgendaAlarmes` dans Android Studio.
3. Laisser Gradle synchroniser le projet.
4. Menu **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
5. L'APK debug se trouve normalement dans `app/build/outputs/apk/debug/app-debug.apk`.

## Notes Android importantes
- La création d'une véritable alarme utilise `AlarmClock.ACTION_SET_ALARM`. Android ouvre l'application Horloge compatible avec l'heure et le libellé préremplis. Selon le téléphone, l'utilisateur doit confirmer l'enregistrement.
- Les notifications demandent l'autorisation Android 13+.
- Android 12+ peut limiter les alarmes exactes selon les réglages d'accès spécial. Le projet utilise un rappel moins précis en solution de repli si l'accès exact n'est pas autorisé.
- Version MVP : stockage local simple (SharedPreferences JSON). Pas encore de synchronisation Google Calendar, modification d'événements existants, vues mois/semaine ni édition d'un événement après création.


## Compilation automatique dans le cloud (GitHub Actions)
Le dossier `.github/workflows/android-apk.yml` permet de compiler l'APK dans GitHub Actions.
Après avoir téléversé le contenu du projet dans un dépôt GitHub, ouvrez l'onglet **Actions** et lancez **Construire APK Android** avec **Run workflow** (ou attendez la compilation déclenchée par le push). Quand l'exécution est terminée, ouvrez-la puis téléchargez l'artefact `MonAgendaAlarmes-APK`.


## Synchronisation du calendrier Outlook (Android)
La version inclut une lecture du calendrier Android via Calendar Provider. Pour afficher les événements Outlook :
1. Dans Outlook Android : Paramètres > sélectionner le compte > Calendrier > activer **Synchroniser les calendriers**.
2. Au premier lancement de Mon Agenda & Alarmes, autoriser l'accès au calendrier.
3. Les événements sont relus automatiquement à chaque ouverture et à chaque retour de l'application au premier plan. L'icône d'actualisation permet aussi de forcer une mise à jour.
Les événements importés sont affichés comme « Événement Outlook » et ne peuvent pas être supprimés depuis l'application. La synchronisation dépend du compte Outlook et de l'export vers le calendrier Android. La lecture seule est utilisée par l'application.


L'actualisation automatique est déclenchée à chaque événement Android `ON_RESUME` (ouverture/reprise de l'application). Elle relit le calendrier local du téléphone ; elle ne déclenche pas elle-même une synchronisation réseau Outlook. La fraîcheur des données dépend donc de la synchronisation Outlook configurée sur Android.


### Création d'une alarme dans Samsung Horloge
L'application demande à Android d'ouvrir l'écran de création d'alarme de l'application Horloge, avec le titre et l'heure calculée. Il faut confirmer/enregistrer l'alarme dans Horloge. Android standard ne permet pas de transmettre une date complète à l'écran de création d'alarme : cette fonction convient aux alarmes quotidiennes à une heure donnée, mais ne garantit pas une alarme unique à une date future. Pour une alerte ponctuelle à une date précise, l'application doit programmer elle-même une alarme Android.


## Correctifs version 1.0.1
- L'ouverture de Samsung Horloge est protégée contre les refus de permission et les applications Horloge absentes.
- En cas de refus de Samsung Horloge, l'application programme un rappel local de secours au lieu de se fermer.
- Le rappel de secours utilise un canal de notification distinct avec sonnerie d'alarme et vibration.
- La programmation et l'annulation des rappels locaux interceptent les erreurs Android afin d'éviter un plantage.
