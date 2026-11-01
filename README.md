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
