# Ex.No: 11
# Develop an application to draw a shapes using 3D graphics with openGL ES.

## AIM:

To create and design an android application to draw a shapes using 3D graphics with openGL ES using Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min. required Arctic Fox)

## ALGORITHM:

Step 1: Open Android Studio and then click on File -> New -> New project.

Step 2: Then type the Application name as “graphics″ and click Next.

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Build an OpenGL ES environment.

Step 6: Create MyGLActivity and MyGLRenderer to add the code.

Step 7: Define the shapes in Cube.java and Pyramid.java

Step 8: Save and run the application.

## PROGRAM:
```
/*
Program to create and design an android application to draw shapes using 3D graphics with openGL ES.
Developed by        : J . RITHANIEPRIYANKA
Registration Number : 212220230039
*/
```
#### MyGLActiviy.java
```
package com.example.graphics;

import androidx.appcompat.app.AppCompatActivity;
import android.opengl.GLSurfaceView;
import android.os.Bundle;

public class MyGLActivity extends AppCompatActivity {

    private GLSurfaceView glView;   

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        glView = new GLSurfaceView(this);
        glView.setRenderer(new MyGLRenderer(this));
        this.setContentView(glView);
    }

    @Override
    protected void onPause() {
        super.onPause();
        glView.onPause();
    }

    @Override
    protected void onResume() {
        super.onResume();
        glView.onResume();
    }
}
```
#### MyGLRenderer.java
```
package com.example.graphics;

import javax.microedition.khronos.egl.EGLConfig;
import javax.microedition.khronos.opengles.GL10;
import android.content.Context;
import android.opengl.GLSurfaceView;
import android.opengl.GLU;

public class MyGLRenderer implements GLSurfaceView.Renderer {

    private Pyramid pyramid;
    private Cube cube;

    private static float anglePyramid = 0;
    private static float angleCube = 0;
    private static float speedPyramid = 2.0f;
    private static float speedCube = -1.5f;

    public MyGLRenderer(Context context) {
        pyramid = new Pyramid();
        cube = new Cube();
    }

    @Override
    public void onSurfaceCreated(GL10 gl, EGLConfig config) {
        gl.glClearColor(0.0f, 0.0f, 0.0f, 1.0f);
        gl.glClearDepthf(1.0f);
        gl.glEnable(GL10.GL_DEPTH_TEST);
        gl.glDepthFunc(GL10.GL_LEQUAL);
        gl.glHint(GL10.GL_PERSPECTIVE_CORRECTION_HINT, GL10.GL_NICEST);
        gl.glShadeModel(GL10.GL_SMOOTH);
        gl.glDisable(GL10.GL_DITHER);
    }

    @Override
    public void onSurfaceChanged(GL10 gl, int width, int height) {
        if (height == 0) height = 1;
        float aspect = (float)width / height;

        gl.glViewport(0, 0, width, height);

        gl.glMatrixMode(GL10.GL_PROJECTION);
        gl.glLoadIdentity();
        GLU.gluPerspective(gl, 45, aspect, 0.1f, 100.f);

        gl.glMatrixMode(GL10.GL_MODELVIEW);
        gl.glLoadIdentity();
    }

    @Override
    public void onDrawFrame(GL10 gl) {
        gl.glClear(GL10.GL_COLOR_BUFFER_BIT | GL10.GL_DEPTH_BUFFER_BIT);

        gl.glLoadIdentity();
        gl.glTranslatef(-1.5f, 0.0f, -6.0f);
        gl.glRotatef(anglePyramid, 0.1f, 1.0f, -0.1f);
        pyramid.draw(gl);

        gl.glLoadIdentity();
        gl.glTranslatef(1.5f, 0.0f, -6.0f);
        gl.glScalef(0.8f, 0.8f, 0.8f);
        gl.glRotatef(angleCube, 1.0f, 1.0f, 1.0f);
        cube.draw(gl);

        anglePyramid += speedPyramid;
        angleCube += speedCube;
    }
}
```
#### Cube.java
```
package com.example.graphics;

import java.nio.ByteBuffer;
import java.nio.ByteOrder;
import java.nio.FloatBuffer;
import javax.microedition.khronos.opengles.GL10;

public class Cube {
    private FloatBuffer vertexBuffer;
    private int numFaces = 6;

    private float[][] colors = {
            {1.0f, 0.5f, 0.0f, 1.0f},
            {1.0f, 0.0f, 1.0f, 1.0f},
            {0.0f, 1.0f, 0.0f, 1.0f},
            {0.0f, 0.0f, 1.0f, 1.0f},
            {1.0f, 0.0f, 0.0f, 1.0f},
            {1.0f, 1.0f, 0.0f, 1.0f}
    };

    private float[] vertices = {
            -1.0f, -1.0f,  1.0f,
            1.0f, -1.0f,  1.0f,
            -1.0f,  1.0f,  1.0f,
            1.0f,  1.0f,  1.0f,

            1.0f, -1.0f, -1.0f,
            -1.0f, -1.0f, -1.0f,
            1.0f,  1.0f, -1.0f,
            -1.0f,  1.0f, -1.0f,

            -1.0f, -1.0f, -1.0f,
            -1.0f, -1.0f,  1.0f,
            -1.0f,  1.0f, -1.0f,
            -1.0f,  1.0f,  1.0f,

            1.0f, -1.0f,  1.0f,
            1.0f, -1.0f, -1.0f,
            1.0f,  1.0f,  1.0f,
            1.0f,  1.0f, -1.0f,

            -1.0f,  1.0f,  1.0f,
            1.0f,  1.0f,  1.0f,
            -1.0f,  1.0f, -1.0f,
            1.0f,  1.0f, -1.0f,

            -1.0f, -1.0f, -1.0f,
            1.0f, -1.0f, -1.0f,
            -1.0f, -1.0f,  1.0f,
            1.0f, -1.0f,  1.0f
    };

    public Cube() {
        ByteBuffer vbb = ByteBuffer.allocateDirect(vertices.length * 4);
        vbb.order(ByteOrder.nativeOrder());
        vertexBuffer = vbb.asFloatBuffer();
        vertexBuffer.put(vertices);
        vertexBuffer.position(0);
    }

    public void draw(GL10 gl) {
        gl.glFrontFace(GL10.GL_CCW);
        gl.glEnable(GL10.GL_CULL_FACE);
        gl.glCullFace(GL10.GL_BACK);

        gl.glEnableClientState(GL10.GL_VERTEX_ARRAY);
        gl.glVertexPointer(3, GL10.GL_FLOAT, 0, vertexBuffer);

        for (int face = 0; face < numFaces; face++) {
            gl.glColor4f(colors[face][0], colors[face][1], colors[face][2], colors[face][3]);
            gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, face*4, 4);
        }
        gl.glDisableClientState(GL10.GL_VERTEX_ARRAY);
        gl.glDisable(GL10.GL_CULL_FACE);
    }
}
```
#### Pyramid.java
```
package com.example.graphics;

import java.nio.ByteBuffer;
import java.nio.ByteOrder;
import java.nio.FloatBuffer;
import javax.microedition.khronos.opengles.GL10;

public class Pyramid {
    private FloatBuffer vertexBuffer;
    private FloatBuffer colorBuffer;
    private ByteBuffer indexBuffer;

    private float[] vertices = {
            -1.0f, -1.0f, -1.0f,
            1.0f, -1.0f, -1.0f,
            1.0f, -1.0f,  1.0f,
            -1.0f, -1.0f,  1.0f,
            0.0f,  1.0f,  0.0f
    };

    private float[] colors = {
            0.0f, 0.0f, 1.0f, 1.0f,
            0.0f, 1.0f, 0.0f, 1.0f,
            0.0f, 0.0f, 1.0f, 1.0f,
            0.0f, 1.0f, 0.0f, 1.0f,
            1.0f, 0.0f, 0.0f, 1.0f
    };

    private byte[] indices = {
            2, 4, 3,
            1, 4, 2,
            0, 4, 1,
            4, 0, 3
    };

    public Pyramid() {
        ByteBuffer vbb = ByteBuffer.allocateDirect(vertices.length * 4);
        vbb.order(ByteOrder.nativeOrder());
        vertexBuffer = vbb.asFloatBuffer();
        vertexBuffer.put(vertices);
        vertexBuffer.position(0);

        ByteBuffer cbb = ByteBuffer.allocateDirect(colors.length * 4);
        cbb.order(ByteOrder.nativeOrder());
        colorBuffer = cbb.asFloatBuffer();
        colorBuffer.put(colors);
        colorBuffer.position(0);

        indexBuffer = ByteBuffer.allocateDirect(indices.length);
        indexBuffer.put(indices);
        indexBuffer.position(0);
    }

    public void draw(GL10 gl) {
        gl.glFrontFace(GL10.GL_CCW);

        gl.glEnableClientState(GL10.GL_VERTEX_ARRAY);
        gl.glVertexPointer(3, GL10.GL_FLOAT, 0, vertexBuffer);
        gl.glEnableClientState(GL10.GL_COLOR_ARRAY);
        gl.glColorPointer(4, GL10.GL_FLOAT, 0, colorBuffer);

        gl.glDrawElements(GL10.GL_TRIANGLES, indices.length, GL10.GL_UNSIGNED_BYTE,
                indexBuffer);

        gl.glDisableClientState(GL10.GL_VERTEX_ARRAY);
        gl.glDisableClientState(GL10.GL_COLOR_ARRAY);
    }
}
```
#### AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.graphics">

    <uses-feature android:glEsVersion="0x00020000" android:required="true"/>

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Graphics"
        tools:targetApi="31">
        <activity
            android:name=".MyGLActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
## OUTPUT:

![image](https://user-images.githubusercontent.com/75234991/171031593-93607f57-2de1-4ae0-8cf6-2f68d73a68c6.png)

![image](https://user-images.githubusercontent.com/75234991/171031619-2e79ee7e-6902-470f-94be-85bb11e4c012.png)

![image](https://user-images.githubusercontent.com/75234991/171031652-7a0bd859-948b-43a4-9819-be40b0231a84.png)

![image](https://user-images.githubusercontent.com/75234991/171031701-91c22a0d-3b7c-4491-baa7-10dde223382b.png)

![image](https://user-images.githubusercontent.com/75234991/171031721-7efd1149-32db-4196-971e-a0dea19d0980.png)

## RESULT:
Thus a Simple Android Application to draw shapes using 3D graphics with openGL ES using Android Studio is developed and executed successfully.
