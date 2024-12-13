# script

public class Movimiento : MonoBehaviour
{
    public float velocidad = 8.0f;
    public float rotacion = 1000.0f;
    public float salto = 5.0f;

    private Rigidbody Fisicas;

    // Start is called before the first frame update
    void Start()
    {
        //Cursor.lockState = CursorLockMode.Locked;
        //Cursor.visible = true;

        Fisicas = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");

        transform.Translate(new Vector3(horizontal, 0.0f, vertical)*Time.deltaTime*velocidad);

        float rotacionM = Input.GetAxis("Mouse X");
        transform.Rotate(new Vector3(0.0f, rotacionM * Time.deltaTime * rotacion,  0.0f));

        if (Input.GetKeyDown(KeyCode.Space) && transform.position.y <= 1.5f)
        {
            Fisicas.AddForce(new Vector3(0, salto, 0), ForceMode.Impulse);
        }
    }
}


public class Agarrar : MonoBehaviour
{
    public GameObject manoDer;
    private GameObject objetoDer = null;

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if (objetoDer != null)
        {
            if (Input.GetKey("r"))
            {
                objetoDer.GetComponent<Rigidbody>().useGravity = true;
                objetoDer.GetComponent <Rigidbody>().isKinematic = false;
                objetoDer.gameObject.transform.SetParent(null);
                objetoDer = null;
            }
        }
    }

    private void OnTriggerStay(Collider other)
    {
        if (other.gameObject.CompareTag("Objeto"))
        {
            if (Input.GetKey("e") && objetoDer == null)
            {
                other.GetComponent<Rigidbody>().useGravity = false;
                other.GetComponent<Rigidbody>().isKinematic = true;
                other.transform.position = manoDer.transform.position;
                other.gameObject.transform.SetParent(manoDer.gameObject.transform);
                objetoDer = other.gameObject;
            }
        }
    }
}
