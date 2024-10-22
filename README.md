using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float turnSpeed = 360f;
    public Camera playerCamera;
    
    private Vector3 moveDirection;

    void Update()
    {
        Move();
        Turn();
    }

    // Handles movement input (WASD or arrow keys)
    void Move()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");

        moveDirection = new Vector3(h, 0, v).normalized;

        if (moveDirection.magnitude >= 0.1f)
        {
            Vector3 move = transform.forward * moveSpeed * Time.deltaTime;
            transform.Translate(move, Space.World);
        }
    }

    // Handles camera and player rotation
    void Turn()
    {
        float mouseX = Input.GetAxis("Mouse X") * turnSpeed * Time.deltaTime;

        transform.Rotate(0, mouseX, 0);
    }
}
<!---
Abhaypotato/Abhaypotato is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
using UnityEngine;

public class CameraFollow : MonoBehaviour
{
    public Transform player;  // Reference to the player's transform
    public Vector3 offset;    // Offset distance between the player and camera

    void LateUpdate()
    {
        Vector3 desiredPosition = player.position + offset;
        transform.position = desiredPosition;
    }
}using UnityEngine;

public class VehicleInteraction : MonoBehaviour
{
    public Transform player;
    public Transform vehicle;

    private bool inVehicle = false;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.E) && !inVehicle)
        {
            EnterVehicle();
        }
        else if (Input.GetKeyDown(KeyCode.E) && inVehicle)
        {
            ExitVehicle();
        }
    }

    void EnterVehicle()
    {
        inVehicle = true;
        player.gameObject.SetActive(false);
        player.position = vehicle.position;
        player.parent = vehicle;
    }

    void ExitVehicle()
    {
        inVehicle = false;
        player.gameObject.SetActive(true);
        player.parent = null;
    }
}using UnityEngine;

public class PlayerShooting : MonoBehaviour
{
    public Transform gunBarrel;  // Position where bullets spawn
    public GameObject bulletPrefab;  // Bullet prefab to instantiate
    public float bulletSpeed = 20f;
    public float fireRate = 0.2f;  // Time between shots

    private float fireCooldown = 0f;

    void Update()
    {
        fireCooldown -= Time.deltaTime;

        if (Input.GetButton("Fire1") && fireCooldown <= 0)
        {
            Shoot();
            fireCooldown = fireRate;
        }
    }

    void Shoot()
    {
        GameObject bullet = Instantiate(bulletPrefab, gunBarrel.position, gunBarrel.rotation);
        Rigidbody rb = bullet.GetComponent<Rigidbody>();
        rb.velocity = gunBarrel.forward * bulletSpeed;

        Destroy(bullet, 3f);  // Bullet disappears after 3 seconds to prevent clutter
    }
}using UnityEngine;

public class Bullet : MonoBehaviour
{
    public float damage = 10f;

    private void OnCollisionEnter(Collision collision)
    {
        // Detect if the bullet hits an enemy or object
        EnemyHealth enemy = collision.gameObject.GetComponent<EnemyHealth>();
        if (enemy != null)
        {
            enemy.TakeDamage(damage);
        }

        // Destroy bullet on impact
        Destroy(gameObject);
    }
}using UnityEngine;
using UnityEngine.AI;  // For navigation (NavMesh system)

public class PoliceAI : MonoBehaviour
{
    public Transform player;
    public float detectionRange = 20f;
    public float attackRange = 10f;
    public float chaseSpeed = 5f;

    private NavMeshAgent agent;
    private bool isChasing = false;

    void Start()
    {
        agent = GetComponent<NavMeshAgent>();
    }

    void Update()
    {
        float distanceToPlayer = Vector3.Distance(player.position, transform.position);

        // Start chasing if the player is within detection range
        if (distanceToPlayer <= detectionRange)
        {
            isChasing = true;
        }

        if (isChasing)
        {
            agent.SetDestination(player.position);

            if (distanceToPlayer <= attackRange)
            {
                // If close enough to player, stop and attack (you can add shooting or melee attack)
                AttackPlayer();
            }
        }
    }

    void AttackPlayer()
    {
        // Placeholder for attack logic (e.g., shooting at player or melee attack)
        Debug.Log("Police attacking player!");
    }
}using UnityEngine;

public class EnemyHealth : MonoBehaviour
{
    public float maxHealth = 100f;
    private float currentHealth;

    void Start()
    {
        currentHealth = maxHealth;
    }

    public void TakeDamage(float damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    void Die()
    {
        // Police death behavior (you can add animations or ragdoll effects)
        Destroy(gameObject);
    }
}using UnityEngine;

public class PlayerHealth : MonoBehaviour
{
    public float maxHealth = 100f;
    private float currentHealth;

    void Start()
    {
        currentHealth = maxHealth;
    }

    public void TakeDamage(float damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    void Die()
    {
        // Player death logic (e.g., respawn, game over screen)
        Debug.Log("Player died");
    }
}using UnityEngine;

public class PlayerHealth : MonoBehaviour
{
    public float maxHealth = 100f;
    private float currentHealth;

    void Start()
    {
        currentHealth = maxHealth;
    }

    public void TakeDamage(float damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    void Die()
    {
        // Player death logic (e.g., respawn, game over screen)
        Debug.Log("Player died");
    }
}using UnityEngine;

public class CrimeSystem : MonoBehaviour
{
    public PoliceAI[] policeUnits;  // Reference to all police units in the scene

    public void CommitCrime()
    {
        foreach (PoliceAI police in policeUnits)
        {
            police.isChasing = true;  // Trigger all police to chase the player
        }
    }
}void Shoot()
{
    GameObject bullet = Instantiate(bulletPrefab, gunBarrel.position, gunBarrel.rotation);
    Rigidbody rb = bullet.GetComponent<Rigidbody>();
    rb.velocity = gunBarrel.forward * bulletSpeed;

    CrimeSystem crimeSystem = FindObjectOfType<CrimeSystem>();
    if (crimeSystem != null)
    {
        crimeSystem.CommitCrime();  // Notify police when shooting occurs
    }

    Destroy(bullet, 3f);  // Bullet disappears after 3 seconds
}
