# Unity 2D Coin Collector Game
# Name:RAMPRASATH R
# REG NO:212223220086
# AIM:
To create a Unity-based 2D game where the player collects coins, avoids obstacles, and reaches the end of the level.

# Procedure:

# Create a New Unity Project

Open Unity Hub

Click New Project

Select 2D Core

Name it → My2DGame

Choose a folder → Create

# Setup the Scene

Delete the default Main Camera if needed and create a new 2D Camera (optional).

Drag and drop your background PNG into the Scene.

Set Order in Layer = -10 so objects appear in front.

# Add Ground / Platforms

Drag ground tile PNG into Scene.

Click it → In Inspector, add:

Sprite Renderer

Box Collider 2D

For stepping platforms, duplicate (Ctrl + D).

# Add Player

Drag the player sprite into Scene.

Add:

Sprite Renderer

Rigidbody2D (Body Type: Dynamic)

Capsule Collider 2D

Create a folder → Scripts

Create script: PlayerMovement.cs

```
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float speed = 5f;
    public float jumpForce = 7f;
    Rigidbody2D rb;
    bool isGrounded;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        float x = Input.GetAxis("Horizontal");
        rb.velocity = new Vector2(x * speed, rb.velocity.y);

        if (Input.GetButtonDown("Jump") && isGrounded)
            rb.AddForce(Vector2.up * jumpForce, ForceMode2D.Impulse);
    }

    void OnCollisionEnter2D(Collision2D col)
    {
        if (col.gameObject.CompareTag("Ground"))
            isGrounded = true;
    }

    void OnCollisionExit2D(Collision2D col)
    {
        if (col.gameObject.CompareTag("Ground"))
            isGrounded = false;
    }
}
```
Attach this script to the player.

Set ground objects → Tag → Ground.

# Add Coins

Drag coin sprite into Scene.

Add:

Circle Collider 2D

Tick Is Trigger

Create script → Coin.cs
```
using UnityEngine;

public class Coin : MonoBehaviour
{
    void OnTriggerEnter2D(Collider2D col)
    {
        if (col.CompareTag("Player"))
            Destroy(gameObject);
    }
}
```
Attach to each coin.

# Add a Score System

Create a UI → Text (TMP) object called ScoreText

Create script → GameManager.cs
```
using UnityEngine;
using TMPro;

public class GameManager : MonoBehaviour
{
    public int score;
    public TextMeshProUGUI scoreText;

    public void AddScore(int v)
    {
        score += v;
        scoreText.text = score.ToString();
    }
}
```

In Coin.cs, change destroy to add score:

FindObjectOfType<GameManager>().AddScore(1);
Destroy(gameObject);

# Add Camera Follow

Create script → CameraFollow.cs
```
using UnityEngine;

public class CameraFollow : MonoBehaviour
{
    public Transform target;
    public float smooth = 0.1f;
    Vector3 v;

    void LateUpdate()
    {
        Vector3 pos = new Vector3(target.position.x, target.position.y, -10);
        transform.position = Vector3.SmoothDamp(transform.position, pos, ref v, smooth);
    }
}
```

Attach to Main Camera and drag the player into target.

# Add Physics Settings

Menu → Edit → Project Settings → Physics 2D

Gravity: -9.81 (default)

Collision Matrix: leave default

# Build the Game

Go to File → Build Settings

Choose PC, Mac & Linux Standalone

Add your scene

Build & Run

# Output:

<img width="1919" height="1077" alt="Screenshot 2025-11-11 143206" src="https://github.com/user-attachments/assets/54932e21-abaa-49c5-a8dd-7b95d1e5b16b" />

<img width="1919" height="1009" alt="Screenshot 2025-11-11 143434" src="https://github.com/user-attachments/assets/796fd1a7-3c42-44ef-ae4a-8ebb83cc2165" />

# Result:
The Unity 2D game was developed successfully, allowing the player to collect coins, move across platforms, and interact with objects smoothly.
