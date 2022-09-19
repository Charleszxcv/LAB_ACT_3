#include <iostream>
#include <gl/glut.h>

using namespace std;

void display();
void reshape(int, int);
void update(int);

float angle = 0; //rotation angle
float x_pos = -9.0f; //x position
int state = 1; //back&fort logic

void init()
{

	glClearColor(0.0, 0.0, 0.0, 1.0); //set background to black

}

int main(int argc, char** argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_DOUBLE); //display modes

	glutInitWindowPosition(200, 100); //window postion
	glutInitWindowSize(500, 500); //window size
	glutCreateWindow("LAB_ACT_3"); //create the initialized window with name

	glutDisplayFunc(display); //initialize display function
	glutReshapeFunc(reshape); //initialize reshape function

	init();

	glutTimerFunc(1000 / 60, update, 0); //add timer

	glutMainLoop();//loop at the functions

}

void display()
{

	glClear(GL_COLOR_BUFFER_BIT);//clear color

	glLoadIdentity();//clear transforms

	glPushMatrix();

	glRotatef(angle, 0.0f, 1.0f, 0.0f); //animated rotation

	glBegin(GL_QUADS);
	glColor3f(1.0f, 0.0f, 0.0f);
	glVertex2f(-8.0f, -5.0f);
	glVertex2f(8.0f, -5.0f);
	glColor3f(0.0f, 0.0f, 1.0f);
	glVertex2f(8.0f, 5.0f);
	glVertex2f(-8.0f, 5.0f);
	glEnd();


	glBegin(GL_TRIANGLES);

	glColor3f(1.0f, 1.0f, 1.0f);
	glVertex2f(-8.0f, -5.0f);
	glVertex2f(-2.0f, 0.0f);
	glVertex2f(-8.0f, 5.0f);
	glEnd();


	
	glPointSize(20);
		glBegin(GL_POINTS);
		glColor3f(1.0f, 1.0f, 0.0f);
		glVertex2f(-6.0f, 0.0f);
		glEnd();


	glPopMatrix();



	glutSwapBuffers(); //Send the scene to the screen
}

void reshape(int w, int h)
{
	glViewport(0, 0, (GLsizei)w, (GLsizei)h);
	glMatrixMode(GL_PROJECTION);
	gluOrtho2D(-10, 10, -10, 10);//size of the world
	glMatrixMode(GL_MODELVIEW);
}

void update(int)
{


	//rotate looping animation logic
	angle += 0.8;
	if (angle > 360.0f)
		angle = angle - 360.0f;


	//translate looping animation logic
	switch (state) {
	case 1:
		if (x_pos < 9)
			x_pos += 0.15f;
		else
			state = -1;
		break;
	case -1:
		if (x_pos > -9)
			x_pos -= 0.15f;
		else
			state = 1;
		break;

	}

	glutPostRedisplay();
	glutTimerFunc(1000 / 60, update, 0);

}
