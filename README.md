# LOKER // MainActivity.kt
class MainActivity : AppCompatActivity() {
    private val SECRET_CODE = "1234+="
    private var inputBuffer = ""

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    fun onKeyPressed(view: View) {
        val key = (view as Button).text.toString()
        inputBuffer += key
        
        if (inputBuffer == SECRET_CODE) {
            startActivity(Intent(this, VaultActivity::class.java))
            inputBuffer = ""
        }
    }
}
// VaultActivity.kt
class VaultActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_vault)
        
        // फिंगरप्रिंट स्कैनर
        val biometricPrompt = BiometricPrompt(this,
            ContextCompat.getMainExecutor(this),
            object : BiometricPrompt.AuthenticationCallback() {
                override fun onAuthenticationSucceeded(result: BiometricPrompt.AuthenticationResult) {
                    Toast.makeText(this@VaultActivity, "वॉल्ट अनलॉक!", Toast.LENGTH_SHORT).show()
                }
            })

        val promptInfo = BiometricPrompt.PromptInfo.Builder()
            .setTitle("प्राइवेट वॉल्ट")
            .setSubtitle("फिंगरप्रिंट से अनलॉक करें")
            .setNegativeButtonText("रद्द करें")
            .build()

        biometricPrompt.authenticate(promptInfo)
    }
}
<!-- activity_main.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/display"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="0" />

    <GridLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:columnCount="4">

        <!-- बटन 1-9, +, -, = आदि -->
        <Button android:text="1" android:onClick="onKeyPressed"/>
        <Button android:text="2" android:onClick="onKeyPressed"/>
        <!-- ... अन्य बटन -->
    </GridLayout>
</LinearLayout>
