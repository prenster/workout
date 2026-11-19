# Workout Timer — workout file format

This describes the workouts the Workout Timer app imports. If you're an AI assistant asked to make a workout for this app: give a short summary, then the workout as **one JSON code block** in exactly this format. The user will paste your whole reply into the app's "Workout from Claude" screen, or save it as a `.json` file and use Import.

## Format

```json
{
  "name": "15-min full body",
  "rounds": 3,
  "prep": 10,
  "roundRest": 60,
  "steps": [
    { "name": "Jumping jacks", "secs": 40, "reps": 0,  "rest": 15 },
    { "name": "Squats",        "secs": 40, "reps": 0,  "rest": 15 },
    { "name": "Push-ups",      "secs": 45, "reps": 12, "rest": 15 },
    { "name": "Lunges",        "secs": 40, "reps": 0,  "rest": 15 },
    { "name": "Plank",         "secs": 30, "reps": 0,  "rest": 0 }
  ]
}
```

## Fields

**Workout**

| Field       | Meaning |
|-------------|---------|
| `name`      | Short title shown in the app. |
| `rounds`    | How many times the whole `steps` list repeats (1 or more). |
| `prep`      | Seconds of "Get ready" countdown at the start. |
| `roundRest` | Seconds of rest between rounds. It replaces the last step's rest between rounds. 0 = use the last step's own rest. |
| `steps`     | The exercises, in order. |

**Each step**

| Field  | Meaning |
|--------|---------|
| `name` | Exercise name. It is read aloud, so use plain words. |
| `secs` | Seconds of work, with a countdown that moves on automatically. **0 = untimed**: the user taps Done when finished. |
| `reps` | Target reps. Shows a tap-to-count counter. **0 = no counter.** |
| `rest` | Seconds of rest after this step. |

Each step needs `secs` or `reps` above 0, or both. With both, she gets a countdown *and* a rep counter, and the step ends when time runs out.

## Timing rules

- Total length = `prep` + every step's `secs` and `rest`, across all rounds, plus `roundRest` between rounds. There is no rest after the very last step.
- Untimed rep steps (`secs` 0) are estimated at about 3 seconds per rep.
- Make the total come out close to the length the user asked for. For a 15-minute workout that's about 900 seconds.

## Several workouts at once

To give several workouts at once, wrap them in a list: `{ "workouts": [ {...}, {...} ] }`.
