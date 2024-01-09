# Trabalho_25957_25980

Projeto realizado por Gabriel Costa (25957) e Rafael Enes (25980) no âmbito da cadeira de Desenvolvimento de Jogos para Plataformas Moveis.

Neste Projeto decidimos criar um jogo de perguntas e respostas em que o objetivo do jogador é acertar no maior número de perguntas pessivel.

No nosso jogo, as perguntas têm o tema de História.

Telas de Sign in e Log in

![image](https://github.com/GabrielCosta123/Trabalho_25957_25980/assets/120459962/4fe9b01c-69f2-49fc-9691-5149237fab94)
![image](https://github.com/GabrielCosta123/Trabalho_25957_25980/assets/120459962/fb4e9672-4467-40fc-bf01-5a806cb263b7)

Telas de Inicio de Jogo e de Perguntas 

![image](https://github.com/GabrielCosta123/Trabalho_25957_25980/assets/120459962/fa290048-3cc0-4b6c-a46c-86edc1e297c0)
![image](https://github.com/GabrielCosta123/Trabalho_25957_25980/assets/120459962/a99f2202-82dd-4fa9-96ce-7e2bc43db7d3)

Tela de Score

![image](https://github.com/GabrielCosta123/Trabalho_25957_25980/assets/120459962/f663952a-2e48-4817-a34e-e07884f48c7f)


O nosso jogo está dividido em várias classes.

Constants

```kotlin

object Constants {
    const val USER_NAME: String = "user_name"
    const val TOTAL_QUESTIONS: String ="total_questions"
    const val CORRECT_ANSWERS: String="correct_answers"
    fun getQuestions(): ArrayList<Question>{
        val questionsList = ArrayList<Question>()

        val que1 = Question("Quem foi o primeiro presidente dos Estados Unidos?",listOf("Thomas Jefferson", "George Washington", "Abraham Lincoln", "John Adams"),2)
        questionsList.add(que1)
        val que2 = Question( "Qual evento marcou o início da Primeira Guerra Mundial em 1914?", listOf("Bombardeio de Pearl Harbor", "Assassinato de Franz Ferdinand", "Tratado de Versalhes", "Queda do Império Austro-Húngaro"),2)
        questionsList.add(que2)
        val que3 = Question("Quem foi o líder da Revolução Russa em 1917?",listOf("Josef Stalin", "Vladimir Lenin", "Leon Trotsky", "Alexander Kerensky"),2)
        questionsList.add(que3)
        val que4 = Question("Qual foi a capital do Império Romano?", listOf("Alexandria", "Atenas", "Constantinopla", "Roma"), 4)
        questionsList.add(que4)
        val que5 = Question("Qual foi o evento que desencadeou a entrada dos Estados Unidos na Segunda Guerra Mundial?", listOf("Ataque a Pearl Harbor", "Batalha de Stalingrado", "Bombas de Hiroshima e Nagasaki", "Invasão da Polônia"), 1)
        questionsList.add(que5)
        val que6 = Question(" Qual foi o tratado que encerrou a Primeira Guerra Mundial em 1919?", listOf("Tratado de Versalhes", "Tratado de Tordesilhas", "Tratado de Brest-Litovski", "Tratado de Paris"),1 )
        questionsList.add(que6)
        val que7 = Question("Quem foi o primeiro faraó do Antigo Egito unificado?", listOf("Ramsés II","Tutancâmon", "Menés", "Cleópatra"),3)
        questionsList.add(que7)
        val que8 = Question( "Quem foi o primeiro rei de Portugal?", listOf("Dom Sancho I", "Dom Afonso I (Afonso Henriques)", "Dom Pedro I", "Dom João I"),2 )
        questionsList.add(que8)
        val que9 = Question("Quem foi o rei responsável pela construção da Universidade de Coimbra em 1290?", listOf("Dom Fernando I", "Dom Dinis", "Dom João III", "Dom Manuel I"), 2)
        questionsList.add(que9)
        val que10 = Question("Quem liderou a Revolução Chinesa e estabeleceu a República Popular da China em 1949?", listOf("Mao Zedong", "Sun Yat-sen", "Chiang Kai-shek", "Deng Xiaoping"), 1)
        questionsList.add(que10)
        return questionsList


    }

} 
```

Log in

```kotlin
package ipca.projeto.projetoquiz

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.google.firebase.auth.FirebaseAuth


class Login:AppCompatActivity(){

    private lateinit var auth: FirebaseAuth

    public override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.log_in)

        auth = FirebaseAuth.getInstance()

        val signintext : TextView = findViewById(R.id.signintextview)
        signintext.setOnClickListener {
            val intent = Intent(this, Signin::class.java)
            startActivity(intent)
        }

        val buttonlogin : Button = findViewById(R.id.buttonlogin)
        buttonlogin.setOnClickListener {
            login()

        }



    }
    private fun login(){
        val email: EditText = findViewById(R.id.emaillogintext)
        val password: EditText = findViewById(R.id.passwordlogintext)

        if(email.text.isEmpty() || password.text.isEmpty()){
            Toast.makeText(this, "Please fill all the fields", Toast.LENGTH_SHORT).show()
            return
        }
        val emailInput = email.text.toString()
        val passwordInput = password.text.toString()

        auth.signInWithEmailAndPassword(emailInput, passwordInput).addOnCompleteListener(this) { task ->
            if(task.isSuccessful){
                val intent = Intent(this, MainActivity::class.java)
                startActivity(intent)
                Toast.makeText(baseContext,"Success",Toast.LENGTH_SHORT).show()

            }else{
                Toast.makeText(baseContext,"Authentication failed",Toast. LENGTH_SHORT).show()
            }
        }
            .addOnFailureListener {
                Toast.makeText(baseContext, "Authentication failed ${it.localizedMessage}", Toast.LENGTH_SHORT).show()
            }
    }
}

```

Sign in

```kotlin
package ipca.projeto.projetoquiz


import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.google.firebase.auth.FirebaseAuth


class Signin: AppCompatActivity() {

    private lateinit var auth: FirebaseAuth


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.sign_in)

        auth = FirebaseAuth.getInstance()

        val buttoncreate : Button = findViewById(R.id.buttoncreate)
        val logintext : TextView = findViewById(R.id.logintextview)
        logintext.setOnClickListener {
            val intent = Intent(this, Login::class.java)
            startActivity(intent)
        }

        buttoncreate.setOnClickListener {
            signup()
        }

    }
    private fun signup(){
        val email = findViewById<EditText>(R.id.emailsignintext)
        val password = findViewById<EditText>(R.id.passwordsignintext)

        if(email.text.isEmpty() || password.text.isEmpty()){
            Toast.makeText(this,"Please fill all fields", Toast.LENGTH_SHORT).show()
            return
        }

        val inputEmail = email.text.toString()
        val inputPassword = password.text.toString()

        auth.createUserWithEmailAndPassword(inputEmail, inputPassword).addOnCompleteListener(this) { task ->
            if (task.isSuccessful) {
                val intent = Intent(this, MainActivity::class.java)
                startActivity(intent)
                Toast.makeText(baseContext,"Success",Toast.LENGTH_SHORT).show()

            }else{
                Toast.makeText(baseContext, "FAILED",Toast.LENGTH_SHORT).show()
            }
        }
            .addOnFailureListener {
                Toast.makeText(this, "Error ocurred ${it.localizedMessage}", Toast.LENGTH_SHORT).show()
            }
    }
}



```

Question

```kotlin
package ipca.projeto.projetoquiz

data class Question(
    val questionText: String,
    val options: List<String>,
    val correctOption: Int
)

```

QuestionActivity

```kotlin
package ipca.projeto.projetoquiz

import android.content.Intent
import android.graphics.Color
import android.graphics.Typeface
import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.core.content.ContextCompat


class QuestionActivity : AppCompatActivity(), View.OnClickListener {

    private var mQuestionsList: ArrayList<Question>? = null
    private var mSelectedOptionPosition: Int = 0
    private var mCurrentPosition: Int = 1
    private lateinit var questiontextview: TextView
    private lateinit var opt1: Button
    private lateinit var opt2: Button
    private lateinit var opt3: Button
    private lateinit var opt4: Button
    private lateinit var submitbutton: Button
    private var mCorrectAnswers: Int = 0
    private var mUserName: String? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.questions_tab)

        mUserName = intent.getStringExtra(Constants.USER_NAME)

        questiontextview = findViewById(R.id.questiontextview)
        opt1 = findViewById(R.id.option1)
        opt2 = findViewById(R.id.option2)
        opt3 = findViewById(R.id.option3)
        opt4 = findViewById(R.id.option4)
        submitbutton = findViewById(R.id.submitbutton)

        mQuestionsList = Constants.getQuestions()

        setQuestion()


    }

    private fun setQuestion() {

        val question = mQuestionsList!![mCurrentPosition - 1]

        defaultOptionsView()
        if (mCurrentPosition == mQuestionsList!!.size) {
            submitbutton.text = "FIM"
        } else {
            submitbutton.text = "Confirmar"
        }
        opt1.setOnClickListener(this)
        opt2.setOnClickListener(this)
        opt3.setOnClickListener(this)
        opt4.setOnClickListener(this)
        submitbutton.setOnClickListener(this)


        questiontextview.text = question!!.questionText
        opt1.text = question!!.options[0]
        opt2.text = question!!.options[1]
        opt3.text = question!!.options[2]
        opt4.text = question!!.options[3]


    }

    private fun defaultOptionsView() {
        val options = ArrayList<Button>()
        options.add(0, opt1)
        options.add(1, opt2)
        options.add(2, opt3)
        options.add(3, opt4)

        for (option in options) {
            option.setTextColor(Color.parseColor("#7A8089"))
            option.typeface = Typeface.DEFAULT

        }
    }

    override fun onClick(v: View?) {
        when (v?.id) {
            R.id.option1 -> {
                selectedOptionView(opt1, 1)
            }

            R.id.option2 -> {
                selectedOptionView(opt2, 2)
            }

            R.id.option3 -> {
                selectedOptionView(opt3, 3)
            }

            R.id.option4 -> {
                selectedOptionView(opt4, 4)
            }

            R.id.submitbutton -> {
                if (mSelectedOptionPosition == 0) {
                    mCurrentPosition++

                    when {
                        mCurrentPosition <= mQuestionsList!!.size -> {
                            setQuestion()
                        }

                        else -> {
                            val intent = Intent(this, ResultActivity::class.java)
                            intent.putExtra(Constants.USER_NAME, mUserName)
                            intent.putExtra(Constants.CORRECT_ANSWERS,mCorrectAnswers)
                            intent.putExtra(Constants.TOTAL_QUESTIONS,mQuestionsList!!.size)
                            startActivity(intent)
                        }
                    }
                } else {
                    val question = mQuestionsList?.get(mCurrentPosition - 1)
                    if (question!!.correctOption != mSelectedOptionPosition) {
                        answerView(mSelectedOptionPosition, Color.RED)
                    } else {
                        mCorrectAnswers++
                    }
                    answerView(question.correctOption, Color.GREEN)
                    if (mCurrentPosition == mQuestionsList!!.size) {
                        submitbutton.text = "FIM"
                    } else {
                        submitbutton.text = " Próxima Pergunta! "
                    }
                    mSelectedOptionPosition = 0
                }
            }
        }
    }


    private fun answerView(answer: Int, textColor: Int) {
        when (answer) {
            1 -> {
                opt1.setTextColor(textColor)
            }

            2 -> {
                opt2.setTextColor(textColor)
            }

            3 -> {
                opt3.setTextColor(textColor)
            }

            4 -> {
                opt4.setTextColor(textColor)
            }
        }
    }

    private fun selectedOptionView(button: Button, selectedOptionNum: Int) {
        defaultOptionsView()
        mSelectedOptionPosition = selectedOptionNum
        button.setTypeface(button.typeface, Typeface.BOLD)


    }
}

```

ResultActivity

```kotlin
package ipca.projeto.projetoquiz

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class ResultActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.score1)

        val username = intent.getStringExtra(Constants.USER_NAME)
        val nome : TextView = findViewById(R.id.nomefimtextview)
        val score : TextView = findViewById(R.id.score)
        val finishbutton : Button = findViewById(R.id.finishButton)
        nome.text = username
        val totalQuestions = intent.getIntExtra(Constants.TOTAL_QUESTIONS,0)
        val correctAnswers = intent.getIntExtra(Constants.CORRECT_ANSWERS,0)
        score.text = "Acertaste $correctAnswers de $totalQuestions perguntas"
        finishbutton.setOnClickListener{
            startActivity(Intent(this,MainActivity::class.java))
        }



    }

}

```

MainActivity

```kotlin
package ipca.projeto.projetoquiz


import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import android.widget.Toast


open class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val playButton: Button = findViewById(R.id.playButton)
        val nometext: TextView = findViewById(R.id.nometextview)

        playButton.setOnClickListener {
            if (nometext.text.toString().isEmpty()) {
                Toast.makeText(this, "Escreve o teu nome", Toast.LENGTH_SHORT).show()
            } else {
                val intent = Intent(this, QuestionActivity::class.java)
                intent.putExtra(Constants.USER_NAME, nometext.text.toString())
                startActivity(intent)
                finish()
            }

        }
    }
}

```
















