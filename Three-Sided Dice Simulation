using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class IN : MonoBehaviour
{
    public GameObject prefab;

    public GameObject[,] coins;
    private int numrows = 50;
    private float spacing = 5f;

    private int[] ratiocount = new int[5];

    private float stillcheck = 0f;
    private float stillchecktimelimit = 0.2f;
    private float throwcheck = 0f;
    private float throwchecktimelimit = 5f;

    private float scale = 0.5f;

    bool running = true;

    int numratio = 20;
    int numrollsperratio = 20;
    int ratioindex = 0;


    private float angvellimit = 6f;
    private float vellimit = 5f;
    private float velstilllimit = 0.2f;
    // Start is called before the first frame update
    void Start()
    {
        coins = new GameObject[numrows, numrows];
        for (int i = 0; i < numrows; i++)
        {
            for (int j = 0; j < numrows; j++)
            {
                coins[i, j] = Instantiate(prefab, new Vector3(Random.Range(-10, 10), 5, Random.Range(-10, 10)), Quaternion.identity);
            }
        }
        throwcoins();
    }
    
    void Update()
    {
        if(running)
        {
            
            stillcheck += Time.deltaTime;
            throwcheck += Time.deltaTime;

            if(stillcheck > stillchecktimelimit)
            {
                stillcheck = 0f;

                if(checkcoinstill() || throwcheck > throwchecktimelimit)
                {
                    recordresult();
                    throwcheck = 0f;

                    if (running) throwcoins();
                }
            }
        }
    }

    public bool checkcoinstill()
    {
        for (int i = 0; i < numrows; i++)
        {
            for (int j = 0; j < numrows; j++)
            {
                if (coins[i, j].transform.position.y > 1f) return false;

                //Rigidbody rb = coins[i, j].GetComponent<Rigidbody>();

                //float vel = Vector3.Magnitude(rb.velocity);
                //float angvel = Vector3.Magnitude(rb.angularVelocity);

                //if (vel > velstilllimit || angvel > velstilllimit) return false;
            }
        }
        return true;
    }
    
    public void recordresult()
    {
        for (int i = 0; i < numrows; i++)
        {
            for (int j = 0; j < numrows; j++)
            {
                float angle = Vector3.Angle(coins[i, j].transform.up, Vector3.up);
                
                if(angle < 5f)
                {
                    Debug.Log("UP");
                    ratiocount[0]++;
                }

                else if(angle > 175f)
                {
                    Debug.Log("Down");
                    ratiocount[1]++;
                }

                else if(Mathf.Abs(angle-90f) < 5f)
                {
                    Debug.Log("Edge");
                    ratiocount[2]++;
                }
            }
        }
        ratiocount[3]++;
        if ((float)ratiocount[2] / (ratiocount[0] + ratiocount[1] + ratiocount[2]) < 0.49f || (float)ratiocount[2] / (ratiocount[0] + ratiocount[1] + ratiocount[2]) > 0.51f)
        {
            Debug.Log((float)ratiocount[2] / (ratiocount[0] + ratiocount[1] + ratiocount[2]));
            for (int i = 0; i < 3; i++) ratiocount[i] = 0;
            if (ratiocount[3] > 100) running = false;
        }
        else
        {
            Debug.Log("\tRatio: " + (float)(ratiocount[2]) / (ratiocount[0] + ratiocount[1] + ratiocount[2]) + "\t\tup: " + ratiocount[0] + "\t\tdown: " + ratiocount[1] + "\t\tedge: " + ratiocount[2]);
            //전체 중 edge로 떨어진 비율
            running = false;
        }
    }

    public void throwcoins()
    {
        for (int i = 0; i < numrows; i++)
        {
            for (int j = 0; j < numrows; j++)
            {
                coins[i, j].transform.position = new Vector3(Random.Range(-10, 10), 5, Random.Range(-10, 10));
                coins[i, j].transform.rotation = Random.rotation;
                coins[i, j].transform.localScale = new Vector3(2, 0.5f + ratiocount[3] / (float)100, 2);
                Rigidbody rb = coins[i, j].GetComponent<Rigidbody>();
                rb.angularVelocity = new Vector3(5, 5, 5);
                rb.velocity = new Vector3(0, 0, 0);
                //기본값 5, 5, 5, 0, 0, 0
            }
        }
    }

    
}
