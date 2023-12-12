# SchoolProject
Here I will put scripts for school projects

Player movement code

using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    private float horizontal; // Stores the horizontal input value.
    [SerializeField]private float speed = 8f; // Speed of the player's movement.
    [SerializeField]private float jumpingPower = 16f; // Jumping force of the player.
    private bool isFacingRight = true; // Flag to check if the player is facing right.

    private bool doubleJump; // Flag to check if the player can perform a double jump.

    [SerializeField] private Rigidbody2D rb; // The Rigidbody2D component attached to the player.
    [SerializeField] private Transform groundCheck; // A Transform to check if the player is grounded.
    [SerializeField] private LayerMask groundLayer; // LayerMask defining what constitutes ground.


    private void Update()
    {
        horizontal = Input.GetAxisRaw("Horizontal"); // Get horizontal input (left/right).

        // Reset double jump when the player is grounded and not holding the jump button.
        if (IsGrounded() && !Input.GetButton("Jump"))
        {
            doubleJump = false;
        }

        // Handle Jump logic.
        if (Input.GetButtonDown("Jump"))
        {
            // Allow jump if the player is grounded or has not used double jump yet.
            if (IsGrounded() || doubleJump)
            {
                rb.velocity = new Vector2(rb.velocity.x, jumpingPower); // Apply jumping force.

                doubleJump = !doubleJump; // Toggle double jump state.
            }
        }

        // Modify jump height based on how long the jump button is held.
        if (Input.GetButtonUp("Jump") && rb.velocity.y > 0f)
        {
            rb.velocity = new Vector2(rb.velocity.x, rb.velocity.y * 0.5f); // Reduce upward velocity.
        }

        Flip(); // Handle flipping the player based on movement direction.
    }

    private void FixedUpdate()
    {
        // Apply horizontal movement to the Rigidbody.
        rb.velocity = new Vector2(horizontal * speed, rb.velocity.y);
    }

    private bool IsGrounded()
    {
        // Check if the player is touching the ground using a circle overlap.
        return Physics2D.OverlapCircle(groundCheck.position, 0.2f, groundLayer);
    }

    private void Flip()
    {
        // Check if player direction changed.
        if (isFacingRight && horizontal < 0f || !isFacingRight && horizontal > 0f)
        {
            Vector3 localScale = transform.localScale;
            isFacingRight = !isFacingRight; // Toggle facing direction.
            localScale.x *= -1f; // Flip the player's sprite by scaling x by -1.
            transform.localScale = localScale;
        }
    }
}



https://github.com/Michael-Shollaj/SchoolProject/assets/55102646/0ed8689a-23dd-49e4-b878-b7087fbd621c



