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
}
