# lab1-4
1.  Ініціалізація OpenGL ES середовища

2. Визначення та прорисовка графічних об’єктів

3. Визначення типу проекції та позиції камери

4. Додавання руху та реагування на дотик

___Лабораторна робота № 1: «Ініціалізація середовища OpenGL ES»___

Визначте графічні примітиви: трикутник 

![Screenshot_21](https://github.com/kabanovV/labs-1-4/assets/152945125/68719d39-a7fb-4892-b2d7-a438dd8ffa56)


___Лабораторна робота № 2: «Визначення графічних об’єктів»___

Позиція камери до:

![Screenshot_21](https://github.com/kabanovV/labs-1-4/assets/152945125/fa5839bc-8083-4487-bba6-411438a3e00a)


Код:

override fun onDrawFrame(gl: GL10) {
        val scratch = FloatArray(16)
        // Redraw background color
        GLES20.glClear(GLES20.GL_COLOR_BUFFER_BIT)
        //mTriangle.draw()


        // Set the camera position (View matrix)
        Matrix.setLookAtM(viewMatrix, 0, 0f, 0f, 6f, 0f, 0f, 0f, 8f, 1.0f, 0.0f)

        // Calculate the projection and view transformation
        Matrix.multiplyMM(vPMatrix, 0, projectionMatrix, 0, viewMatrix, 0)
        //mCube.draw(vPMatrix)
        //mSquare.draw(vPMatrix)
        //mTriangle.draw(vPMatrix)
        // Create a rotation transformation for the triangle
//        val time = SystemClock.uptimeMillis() % 4000L
//        this.angle = 0.090f * time.toInt()

        Matrix.setRotateM(rotationMatrix, 0, angle, 0f, 0f, -1.0f)

        // Combine the rotation matrix with the projection and camera view
        // Note that the vPMatrix factor *must be first* in order
        // for the matrix multiplication product to be correct.
        Matrix.multiplyMM(scratch, 0, vPMatrix, 0, rotationMatrix, 0)

        // Draw triangle
        mTriangle.draw(scratch)
    }

Позиція камери після:

![Screenshot_29](https://github.com/kabanovV/labs-1-4/assets/152945125/eea33604-9d4d-4933-aeaa-1a86f68f416a)



__Лабораторна робота № 3: «Обертання зображення»__

Додавання руху

Код:

override fun onDrawFrame(gl: GL10) {
        val scratch = FloatArray(16)
        
        GLES20.glClear(GLES20.GL_COLOR_BUFFER_BIT)      
        Matrix.setLookAtM(viewMatrix, 0, 0f, 0f, 6f, 0f, 0f, 0f, 0f, 1.0f, 0.0f)

        
        Matrix.multiplyMM(vPMatrix, 0, projectionMatrix, 0, viewMatrix, 0)

        Matrix.setRotateM(rotationMatrix, 0, angle, 0f, 0f, -1.0f)

        Matrix.multiplyMM(scratch, 0, vPMatrix, 0, rotationMatrix, 0)

        mTriangle.draw(scratch)
    }

___Лабораторна робота № 4: «Реагування на дотик»___

override fun onTouchEvent(e: MotionEvent): Boolean {

    val x: Float = e.x
    val y: Float = e.y

    when (e.action) {
        MotionEvent.ACTION_MOVE -> {

            var dx: Float = x - previousX
            var dy: Float = y - previousY

            if (y > height / 2) {
                dx *= -1
            }

            if (x < width / 2) {
                dy *= -1
            }

            renderer.angle += (dx + dy) * TOUCH_SCALE_FACTOR
            requestRender()
        }
    }

    previousX = x
    previousY = y
    return true
}

Результат коду:

Рух камери за дотиком

![Screenshot_23](https://github.com/kabanovV/labs-1-4/assets/152945125/cd121c1b-e6b6-41ba-b95f-3727d51aa46f)



