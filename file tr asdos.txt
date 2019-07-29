#include <windows.h>
#include <stdlib.h>
#include <glut.h>
#include <gl/GL.h>
#include <iostream>
#include "imageloader.h"

const float PI = 3.14159;
float teta;
int is_depth;
float xrot = 0.0f;
float yrot = 0.0f;
float xdiff = 0.0f;
float ydiff = 0.0f;
bool mouseDown = false;
float xawal;
using namespace std;

GLuint _textureId;
GLuint _textureIdlogo;
GLuint _textureIdjendelabolongbolong;


GLuint loadTexture(Image* image) {
	GLuint textureId;
	glGenTextures(1, &textureId);
	glBindTexture(GL_TEXTURE_2D, textureId);
	glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, image->width, image->height, 0, GL_RGB, GL_UNSIGNED_BYTE, image->pixels);
	return textureId;
}

void initRendering() {
	glEnable(GL_DEPTH_TEST);
	//glEnable(GL_LIGHTING);
	//glEnable(GL_LIGHT0);
	glEnable(GL_NORMALIZE);
	glEnable(GL_COLOR_MATERIAL);

	Image* image = loadBMP("aaaaaaaa.bmp");
	_textureId = loadTexture(image);
	image = loadBMP("logo.bmp");
	_textureIdlogo = loadTexture(image);
	image = loadBMP("jendelabolong.bmp");
	_textureIdjendelabolongbolong = loadTexture(image);
	delete image;
}

//insialisasi biar bisa dipangil dimanapun
void atep();
void atasnyaatep();
void bawahan();
void dindingdepanrumah();
void buatdinding();
void buattangga();
void summontiang();
void buatpersegipanjang(GLfloat x, GLfloat xlebar, GLfloat z, GLfloat zpanjang, GLfloat y, GLfloat ytingi, GLfloat r, GLfloat g, GLfloat b);
void buatsegitiga(GLfloat x, GLfloat xlebar, GLfloat z, GLfloat zpanjang, GLfloat y, GLfloat ytingi, GLfloat r, GLfloat g, GLfloat b);
void buatjendela(GLfloat x, GLfloat xlebar, GLfloat z, GLfloat ztingi, GLfloat y, GLfloat ypanjang);
void buatkotak(GLfloat x, GLfloat y, GLfloat z, GLfloat lebar, GLfloat tinggi);


void Inisialisasi(void)
{
	glClearColor(0.0, 0.0, 0.0, 1.0);
	glMatrixMode(GL_PROJECTION);
	//glEnable(GL_LIGHTING);
	glEnable(GL_COLOR_MATERIAL);
	glEnable(GL_DEPTH_TEST);
	glEnable(GL_BLEND);
	glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
	is_depth = 1;

	glEnable(GL_LIGHT0);
	GLfloat light1_diffuse[] = { 1.0, 1.0, 1.0, 1.0 };
	GLfloat light1_specular[] = { 1.0, 1.0, 1.0, 1.0 };
	glLightfv(GL_LIGHT0, GL_DIFFUSE, light1_diffuse);
	glLightfv(GL_LIGHT0, GL_SPECULAR, light1_specular);
	GLfloat light1_position[] = { 0, 1500, 3000, 1.0 };
	glLightfv(GL_LIGHT0, GL_POSITION, light1_position);

	//glEnable(GL_LIGHT1);
	//glLightfv(GL_LIGHT1, GL_DIFFUSE, light1_diffuse);
	//glLightfv(GL_LIGHT1, GL_SPECULAR, light1_specular);
	//light1_position[0] = -1000;
	//light1_position[1] = 1500;
	//light1_position[2] = 3000;
	//light1_position[3] = 1.0;
	//glLightfv(GL_LIGHT1, GL_POSITION, light1_position);

	//glEnable(GL_LIGHT2);
	//glLightfv(GL_LIGHT2, GL_DIFFUSE, light1_diffuse);
	//glLightfv(GL_LIGHT2, GL_SPECULAR, light1_specular);
	//light1_position[0] = 1000;
	//light1_position[1] = -1500;
	//light1_position[2] = 3000;
	//light1_position[3] = 1.0;
	//glLightfv(GL_LIGHT2, GL_POSITION, light1_position);

	//glEnable(GL_LIGHT3);
	//glLightfv(GL_LIGHT3, GL_DIFFUSE, light1_diffuse);
	//glLightfv(GL_LIGHT3, GL_SPECULAR, light1_specular);
	//light1_position[0] = -1000;
	//light1_position[1] = -1500;
	//light1_position[2] = 3000;
	//light1_position[3] = 1.0;
	//glLightfv(GL_LIGHT3, GL_POSITION, light1_position);


}
void buatsilinder(GLfloat x, GLfloat y, GLfloat z, GLfloat radius, GLfloat tinggi) {
	int i;
	int lineAmount = 360; //# of triangles used to draw circle

	//GLfloat radius = 0.8f; //radius
	GLfloat twicePi = 2.0f * PI;
	float sebelumx = 0;
	float sebelumy = 0;
	int cek = 0;
	for (i = 0; i <= lineAmount; i++) {
		glBegin(GL_POLYGON);
		glColor3f(0.4588, 0.349, 0.3059);
		float hitungx = x + (radius * cos(i * twicePi / lineAmount));
		float hitungy = y + (radius * sin(i * twicePi / lineAmount));
		float hitungz = z + tinggi;
		if (cek != 1) {
			cek = 1;
		}
		else {
			glVertex3f(sebelumx, sebelumy, z);
			glVertex3f(sebelumx, sebelumy, z + tinggi);
		}
		glVertex3f(hitungx, hitungy, z + tinggi);
		glVertex3f(hitungx, hitungy, z);
		sebelumx = hitungx;
		sebelumy = hitungy;
		glEnd();
	}
	glBegin(GL_POLYGON);
	glColor3f(0.4588, 0.349, 0.3059);
	for (int i = 0; i < 360; i++)
	{
		teta = i * 3.142 / 180;
		glVertex3f(x + (radius * cos(i * twicePi / lineAmount)), y + (radius * sin(i * twicePi / lineAmount)), z);
	}
	glEnd();
	glBegin(GL_POLYGON);
	glColor3f(0.4588, 0.349, 0.3059);
	for (int i = 0; i < 360; i++)
	{
		teta = i * 3.142 / 180;
		glVertex3f(x + (radius * cos(i * twicePi / lineAmount)), y + (radius * sin(i * twicePi / lineAmount)), z + tinggi);
	}
	glEnd();
}
void buatkotak(GLfloat x, GLfloat y, GLfloat z, GLfloat lebar, GLfloat tinggi) {
	lebar = lebar / 2;
	float xlebarplus = x + lebar;
	float xlebarmin = x - lebar;
	float ztingi = z + tinggi;
	float ylebarplus = y + lebar;
	float ylebarmin = y - lebar;


	glBegin(GL_POLYGON);
	glColor3d(0.4706, 0.3686, 0.3098);
	glVertex3d(xlebarplus, ylebarplus, z);
	glVertex3d(xlebarmin, ylebarplus, z);
	glVertex3d(xlebarmin, ylebarplus, ztingi);
	glVertex3d(xlebarplus, ylebarplus, ztingi);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(0.4706, 0.3686, 0.3098);
	glVertex3d(xlebarplus, ylebarmin, z);
	glVertex3d(xlebarmin, ylebarmin, z);
	glVertex3d(xlebarmin, ylebarmin, ztingi);
	glVertex3d(xlebarplus, ylebarmin, ztingi);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(0.4706, 0.3686, 0.3098);
	glVertex3f(xlebarplus, ylebarplus, z);
	glVertex3f(xlebarmin, ylebarplus, z);
	glVertex3f(xlebarmin, ylebarmin, z);
	glVertex3f(xlebarplus, ylebarmin, z);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(0.4706, 0.3686, 0.3098);
	glVertex3f(xlebarplus, ylebarplus, ztingi);
	glVertex3f(xlebarmin, ylebarplus, ztingi);
	glVertex3f(xlebarmin, ylebarmin, ztingi);
	glVertex3f(xlebarplus, ylebarmin, ztingi);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(0.4706, 0.3686, 0.3098);
	glVertex3f(xlebarplus, ylebarplus, ztingi);
	glVertex3f(xlebarplus, ylebarmin, ztingi);
	glVertex3f(xlebarplus, ylebarmin, z);
	glVertex3f(xlebarplus, ylebarplus, z);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(0.4706, 0.3686, 0.3098);
	glVertex3f(xlebarmin, ylebarplus, ztingi);
	glVertex3f(xlebarmin, ylebarmin, ztingi);
	glVertex3f(xlebarmin, ylebarmin, z);
	glVertex3f(xlebarmin, ylebarplus, z);
	glEnd();
}
void buatpersegipanjang(GLfloat x, GLfloat xlebar, GLfloat z, GLfloat zpanjang, GLfloat y, GLfloat ytingi, GLfloat r, GLfloat g, GLfloat b) {
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(xlebar, ytingi, z);
	glVertex3d(x, ytingi, z);
	glVertex3d(x, ytingi, zpanjang);
	glVertex3d(xlebar, ytingi, zpanjang);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(xlebar, y, z);
	glVertex3d(x, y, z);
	glVertex3d(x, y, zpanjang);
	glVertex3d(xlebar, y, zpanjang);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3f(xlebar, ytingi, z);
	glVertex3f(x, ytingi, z);
	glVertex3f(x, y, z);
	glVertex3f(xlebar, y, z);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3f(xlebar, ytingi, zpanjang);
	glVertex3f(x, ytingi, zpanjang);
	glVertex3f(x, y, zpanjang);
	glVertex3f(xlebar, y, zpanjang);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3f(xlebar, ytingi, zpanjang);
	glVertex3f(xlebar, y, zpanjang);
	glVertex3f(xlebar, y, z);
	glVertex3f(xlebar, ytingi, z);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3f(x, ytingi, zpanjang);
	glVertex3f(x, y, zpanjang);
	glVertex3f(x, y, z);
	glVertex3f(x, ytingi, z);
	glEnd();
}
void buatjendela(GLfloat x, GLfloat xlebar, GLfloat z, GLfloat ztingi, GLfloat y, GLfloat ypanjang) {
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureId);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3d(xlebar, ypanjang, z);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3d(x, ypanjang, z);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3d(x, ypanjang, ztingi);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3d(xlebar, ypanjang, ztingi);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureId);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3d(xlebar, y, z);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3d(x, y, z);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3d(x, y, ztingi);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3d(xlebar, y, ztingi);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureId);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(xlebar, ypanjang, z);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(x, ypanjang, z);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(x, y, z);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(xlebar, y, z);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureId);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(xlebar, ypanjang, ztingi);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(x, ypanjang, ztingi);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(x, y, ztingi);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(xlebar, y, ztingi);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureId);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(xlebar, ypanjang, ztingi);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(xlebar, y, ztingi);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(xlebar, y, z);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(xlebar, ypanjang, z);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureId);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(x, ypanjang, ztingi);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(x, y, ztingi);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(x, y, z);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(x, ypanjang, z);
	glEnd();
	glDisable(GL_TEXTURE_2D);
}
void buatjendelabolongbolong(GLfloat x, GLfloat xlebar, GLfloat z, GLfloat ztingi, GLfloat y, GLfloat ypanjang) {
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureIdjendelabolongbolong);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3d(xlebar, ypanjang, z);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3d(x, ypanjang, z);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3d(x, ypanjang, ztingi);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3d(xlebar, ypanjang, ztingi);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureIdjendelabolongbolong);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3d(xlebar, y, z);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3d(x, y, z);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3d(x, y, ztingi);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3d(xlebar, y, ztingi);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureIdjendelabolongbolong);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(xlebar, ypanjang, z);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(x, ypanjang, z);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(x, y, z);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(xlebar, y, z);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureIdjendelabolongbolong);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(xlebar, ypanjang, ztingi);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(x, ypanjang, ztingi);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(x, y, ztingi);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(xlebar, y, ztingi);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureIdjendelabolongbolong);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(xlebar, ypanjang, ztingi);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(xlebar, y, ztingi);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(xlebar, y, z);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(xlebar, ypanjang, z);
	glEnd();

	glDisable(GL_TEXTURE_2D);
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureIdjendelabolongbolong);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3f(x, ypanjang, ztingi);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3f(x, y, ztingi);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3f(x, y, z);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3f(x, ypanjang, z);
	glEnd();
	glDisable(GL_TEXTURE_2D);
}
void buatsegitiga(GLfloat x, GLfloat xlebar, GLfloat z, GLfloat zpanjang, GLfloat y, GLfloat ytingi, GLfloat r, GLfloat g, GLfloat b) {
	//segitiga
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, z);
	glVertex3d(xlebar, y, z);
	glVertex3d(xlebar, y, zpanjang);
	glEnd();
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, ytingi, z);
	glVertex3d(xlebar, ytingi, z);
	glVertex3d(xlebar, ytingi, zpanjang);
	glEnd();
	//miring
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3f(xlebar, ytingi, zpanjang);
	glVertex3f(x, ytingi, z);
	glVertex3f(x, y, z);
	glVertex3f(xlebar, y, zpanjang);
	glEnd();
	//tidur
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3f(xlebar, ytingi, z);
	glVertex3f(x, ytingi, z);
	glVertex3f(x, y, z);
	glVertex3f(xlebar, y, z);
	glEnd();
	//tegak
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3f(xlebar, ytingi, zpanjang);
	glVertex3f(xlebar, y, zpanjang);
	glVertex3f(xlebar, y, z);
	glVertex3f(xlebar, ytingi, z);
	glEnd();
}
void buatzigzagoon(GLfloat x, GLfloat xlebar, GLfloat zatas, GLfloat zatasatas, GLfloat zbawah, GLfloat zbawahbawah, GLfloat y, GLfloat ytingi, GLfloat r, GLfloat g, GLfloat b) {
	//depan
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatas);
	glVertex3d(x, y, zatasatas);
	glVertex3d(xlebar, y, zbawah);
	glVertex3d(xlebar, y, zbawahbawah);
	glEnd();
	//belakang
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, ytingi, zatas);
	glVertex3d(x, ytingi, zatasatas);
	glVertex3d(xlebar, ytingi, zbawah);
	glVertex3d(xlebar, ytingi, zbawahbawah);
	glEnd();
	//atas
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatasatas);
	glVertex3d(xlebar, y, zbawah);
	glVertex3d(xlebar, ytingi, zbawah);
	glVertex3d(x, ytingi, zatasatas);
	glEnd();
	//bawah
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatas);
	glVertex3d(xlebar, y, zbawahbawah);
	glVertex3d(xlebar, ytingi, zbawahbawah);
	glVertex3d(x, ytingi, zatas);
	glEnd();
	//kanan
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(xlebar, y, zbawah);
	glVertex3d(xlebar, y, zbawahbawah);
	glVertex3d(xlebar, ytingi, zbawahbawah);
	glVertex3d(xlebar, ytingi, zbawah);
	glEnd();
	//kiri
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatas);
	glVertex3d(x, y, zatasatas);
	glVertex3d(x, ytingi, zatasatas);
	glVertex3d(x, ytingi, zatas);
	glEnd();
}
void buatzigzagoon2(GLfloat x, GLfloat xlebar, GLfloat zatas, GLfloat zatasatas, GLfloat zbawah, GLfloat zbawahbawah, GLfloat y, GLfloat ytingi, GLfloat r, GLfloat g, GLfloat b) {
	//depan
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatas);
	glVertex3d(x, y, zatasatas);
	glVertex3d(x, ytingi, zbawah);
	glVertex3d(x, ytingi, zbawahbawah);
	glEnd();
	//belakang
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(xlebar, y, zatas);
	glVertex3d(xlebar, y, zatasatas);
	glVertex3d(xlebar, ytingi, zbawah);
	glVertex3d(xlebar, ytingi, zbawahbawah);
	glEnd();
	//atas
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatasatas);
	glVertex3d(x, ytingi, zbawah);
	glVertex3d(xlebar, ytingi, zbawah);
	glVertex3d(xlebar, y, zatasatas);
	glEnd();
	//bawah
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatas);
	glVertex3d(x, ytingi, zbawahbawah);
	glVertex3d(xlebar, ytingi, zbawahbawah);
	glVertex3d(xlebar, y, zatas);
	glEnd();
	//kanan
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, ytingi, zbawah);
	glVertex3d(x, ytingi, zbawahbawah);
	glVertex3d(xlebar, ytingi, zbawahbawah);
	glVertex3d(xlebar, ytingi, zbawah);
	glEnd();
	//kiri
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zatas);
	glVertex3d(x, y, zatasatas);
	glVertex3d(xlebar, y, zatasatas);
	glVertex3d(xlebar, y, zatas);
	glEnd();
}
void buatzigzagoonultimate(GLfloat x, GLfloat xlebar, GLfloat zatas, GLfloat zatasatas, GLfloat zbawah, GLfloat zbawahbawah, GLfloat y, GLfloat ytingi, GLfloat r, GLfloat g, GLfloat b) {
	//depan
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zbawah);
	glVertex3d(x, y, zbawahbawah);
	glVertex3d(xlebar, y, zbawahbawah);
	glVertex3d(xlebar, y, zbawah);
	glEnd();
	//belakang
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, ytingi, zbawah);
	glVertex3d(x, ytingi, zbawahbawah);
	glVertex3d(xlebar, ytingi, zatas);
	glVertex3d(xlebar, ytingi, zatasatas);
	glEnd();
	//kiri
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zbawah);
	glVertex3d(x, y, zbawahbawah);
	glVertex3d(x, ytingi, zbawahbawah);
	glVertex3d(x, ytingi, zbawah);
	glEnd();
	//kanan
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(xlebar, y, zbawahbawah);
	glVertex3d(xlebar, y, zbawah);
	glVertex3d(xlebar, ytingi, zatasatas);
	glVertex3d(xlebar, ytingi, zatas);
	glEnd();
	//atas
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zbawah);
	glVertex3d(x, ytingi, zbawah);
	glVertex3d(xlebar, ytingi, zatasatas);
	glVertex3d(xlebar, y, zbawah);
	glEnd();

	//atas
	glBegin(GL_POLYGON);
	glColor3d(r, g, b);
	glVertex3d(x, y, zbawahbawah);
	glVertex3d(x, ytingi, zbawahbawah);
	glVertex3d(xlebar, ytingi, zatas);
	glVertex3d(xlebar, y, zbawahbawah);
	glEnd();
}

void atep() {
	//atep dinding belakang
	buatpersegipanjang(-125, 125, 420, 545, 997.5, 1011.25, 0.40, 0.3137, 0.2627);

	//atep dinding belakang kiri
	buatsegitiga(-386.25, -125, 420, 545, 997.5, 1011.25, 0.40, 0.3137, 0.2627);

	//atep dinding belakang kanan
	buatsegitiga(386.25, 125, 420, 545, 997.5, 1011.25, 0.40, 0.3137, 0.2627);

	//atep atas depan
	buatpersegipanjang(-125, 125, 420, 545, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	//atep atas depan kiri
	buatsegitiga(-377.5, -125, 420, 545, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	//atep dinding depan kanan
	buatsegitiga(377.5, 125, 420, 545, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	//atep rumah tengah
	buatpersegipanjang(125, -125, 545, 550, -661.25, 1011.25, 0.2275, 0.1804, 0.1451);

	//atep miring rumah atas kanan
	buatzigzagoon(125, 561.25, 545, 550, 337, 342, -661.25, 1011.25, 0.2275, 0.1804, 0.1451);

	//atep miring rumah atas kiri
	buatzigzagoon(-125, -561.25, 545, 550, 337, 342, -661.25, 1011.25, 0.2275, 0.1804, 0.1451);

	//atep depan teras
	buatpersegipanjang(386.25, -386.25, 419.9, 420, -2.5, -250, 0.4549, 0.3451, 0.3294);
	buatpersegipanjang(386.25, -386.25, 419.9, 420, -250, -657.5, 0.4549, 0.3451, 0.3294);

	//atep atas depan teras
	buatpersegipanjang(-125, 125, 420, 545, -661.25, -657.5, 0.40, 0.3137, 0.2627);

	//atep atas depan kiri teras
	buatsegitiga(-386.25, -125, 420, 545, -647.5, -661.25, 0.40, 0.3137, 0.2627);

	//atep dinding depan kanan teras
	buatsegitiga(386.25, 125, 420, 545, -647.5, -661.25, 0.40, 0.3137, 0.2627);

	//ekstensi
	//miring depan
	buatzigzagoon2(125, -125, 550, 545, 337, 342, -661.25, -911.25, 0.2275, 0.1804, 0.1451);

	//miring belakang
	buatzigzagoon2(125, -125, 550, 545, 337, 342, 1011.25, 1261.25, 0.2275, 0.1804, 0.1451);

	//miring depan kiri
	buatzigzagoonultimate(-561.25, -125, 545, 550, 337, 342, -911.25, -661.25, 0.2275, 0.1804, 0.1451);

	//miring depan kanan
	buatzigzagoonultimate(561.25, 125, 545, 550, 337, 342, -911.25, -661.25, 0.2275, 0.1804, 0.1451);


	//miring belakang kiri
	buatzigzagoonultimate(-561.25, -125, 545, 550, 337, 342, 1261.25, 1011.25, 0.2275, 0.1804, 0.1451);

	//miring belakang kanan
	buatzigzagoonultimate(561.25, 125, 545, 550, 337, 342, 1261.25, 1011.25, 0.2275, 0.1804, 0.1451);
}
void atasnyaatep() {
	//belakang
	buatsegitiga(-177.5, 0, 549.1, 800, 997.5, 1002.5, 0.40, 0.3137, 0.2627);
	buatsegitiga(177.5, 0, 549.1, 800, 997.5, 1002.5, 0.40, 0.3137, 0.2627);

	//depan
	buatsegitiga(-177.5, 0, 549.1, 800, -661.25, -660.75, 0.40, 0.3137, 0.2627);
	buatsegitiga(177.5, 0, 549.1, 800, -661.25, -660.75, 0.40, 0.3137, 0.2627);

	//lantai atep gede
	buatpersegipanjang(-177.5, 177.5, 549.1, 555, -661.25, 1002.5, 0.40, 0.3137, 0.2627);

	//atep miring rumah atas kanan
	buatzigzagoon(0, 200, 800, 810, 515, 525, -681.25, 1031.25, 0.45, 0.3137, 0.2627);
	//atep miring rumah atas kiri
	buatzigzagoon(0, -200, 800, 810, 515, 525, -681.25, 1031.25, 0.45, 0.3137, 0.2627);

	//atep kecil
	//belakang
	buatsegitiga(-130, 0, 680, 850, 997.5, 1002.5, 0.40, 0.3137, 0.2627);
	buatsegitiga(130, 0, 680, 850, 997.5, 1002.5, 0.40, 0.3137, 0.2627);

	//depan
	buatsegitiga(-130, 0, 680, 850, -661.25, -660.75, 0.40, 0.3137, 0.2627);
	buatsegitiga(130, 0, 680, 850, -661.25, -660.75, 0.40, 0.3137, 0.2627);

	//lantai atep kecil
	buatpersegipanjang(-130, 130, 680, 685, -661.25, 1002.5, 0.40, 0.3137, 0.2627);

	//atep miring rumah atas kanan
	buatzigzagoon(0, 150, 850, 860, 650, 660, -681.25, 1031.25, 0.45, 0.3137, 0.2627);
	//atep miring rumah atas kiri
	buatzigzagoon(0, -150, 850, 860, 650, 660, -681.25, 1031.25, 0.45, 0.3137, 0.2627);

	//jendela depan
	buatjendela(-55, 55, 555, 670, -661.26, -660.74);
	buatpersegipanjang(-60, -55, 550, 675, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(60, 55, 550, 675, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 550, 555, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 670, 675, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-5, 5, 555, 670, -661.3, -660.7, 0.5804, 0.4549, 0.3804);

	//jendela bolong bolong depan
	buatjendelabolongbolong(-55, 55, 680, 710, -661.26, -660.74);
	buatpersegipanjang(-60, -55, 675, 715, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(60, 55, 675, 715, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 675, 680, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 710, 715, -661.3, -660.7, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-5, 5, 680, 675, -661.3, -660.7, 0.5804, 0.4549, 0.3804);

	//jendela belakang
	buatjendela(-55, 55, 555, 670, 997.49, 1002.51);
	buatpersegipanjang(-60, -55, 550, 675, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(60, 55, 550, 675, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 550, 555, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 670, 675, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-5, 5, 555, 670, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);

	//jendela bolong bolong belakang
	buatjendelabolongbolong(-55, 55, 680, 710, 997.49, 1002.51);
	buatpersegipanjang(-60, -55, 675, 715, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(60, 55, 675, 715, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 675, 680, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-55, 55, 710, 715, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-5, 5, 680, 675, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);

}
void bawahan() {
	//tanah 
	buatpersegipanjang(600, -600, -450, -400, 1100, -900, 0, 1, 0);

	//bawah rumah
	buatpersegipanjang(375, -375, 0, 20, 1002.5, -250, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(125, -125, 0, 20, -250, -645, 0.2275, 0.1804, 0.1451);

	//bawahnya bawah rumah
	buatpersegipanjang(375, -375, -20, 0, 1002.5, -250, 0.4549, 0.3451, 0.3294);
	buatpersegipanjang(125, -125, -20, 0, -250, -645, 0.4549, 0.3451, 0.3294);

	//buatlogo
	glEnable(GL_TEXTURE_2D);
	glBindTexture(GL_TEXTURE_2D, _textureIdlogo);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

	glBegin(GL_POLYGON);
	glTexCoord2f(0.0f, 0.0f);
	glNormal3f(0.0, 1.0f, 0.0f);
	glVertex3d(-125, -645.1, -140);
	glTexCoord2f(1.0f, 0.0f);
	glVertex3d(125, -645.1, -140);
	glTexCoord2f(1.0f, 1.0f);
	glVertex3d(125, -645.1, -40);
	glTexCoord2f(0.0f, 1.0f);
	glVertex3d(-125, -645.1, -40);
	glEnd();

	glDisable(GL_TEXTURE_2D);

	//connector tiang 1 dan 2
	buatpersegipanjang(370, 380, -30, 10, -638.75, -261.25, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-370, -380, -30, 10, -638.75, -261.25, 0.2275, 0.1804, 0.1451);
}
void pagar() {
	//pagar depan
	buatpersegipanjang(125, -125, 150, 125, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(125, -125, 40, 20, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-80, -95, 150, 20, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-50, -65, 150, 20, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-20, -35, 150, 20, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(10, -5, 150, 20, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(40, 25, 150, 20, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(70, 55, 150, 20, -650, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(100, 85, 150, 20, -650, -645, 0.1, 0.1804, 0.1451);

	//pagar tengah samping kanan
	buatpersegipanjang(125, 125, 150, 125, -440, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(125, 125, 40, 20, -440, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(125, 125, 40, 125, -590, -615, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(125, 125, 40, 125, -545, -570, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(125, 125, 40, 125, -500, -525, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(125, 125, 40, 125, -455, -480, 0.1, 0.1804, 0.1451);

	//pagar tengah samping kiri
	buatpersegipanjang(-125, -125, 150, 125, -440, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-125, -125, 40, 20, -440, -645, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-125, -125, 40, 125, -590, -615, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-125, -125, 40, 125, -545, -570, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-125, -125, 40, 125, -500, -525, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-125, -125, 40, 125, -455, -480, 0.1, 0.1804, 0.1451);

	//pagar kanan kiri belakang
	buatpersegipanjang(-125, -375, 150, 125, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(375, 125, 150, 125, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-125, -375, 40, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(375, 125, 40, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-155, -175, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-185, -205, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-215, -235, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-245, -265, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-275, -295, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-305, -325, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-335, -355, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(175, 155, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(205, 185, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(235, 215, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(265, 245, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(295, 275, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(325, 305, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(355, 335, 150, 20, -250, -250, 0.1, 0.1804, 0.1451);

	//pagar samping kanan belakang
	buatpersegipanjang(375, 375, 150, 125, 0, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(375, 375, 40, 20, 0, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(375, 375, 40, 125, -190, -220, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(375, 375, 40, 125, -140, -170, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(375, 375, 40, 125, -90, -120, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(375, 375, 40, 125, -40, -70, 0.1, 0.1804, 0.1451);

	//pagar samping kiri belakang
	buatpersegipanjang(-375, -375, 150, 125, 0, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-375, -375, 40, 20, 0, -250, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-375, -375, 40, 125, -190, -220, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-375, -375, 40, 125, -140, -170, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-375, -375, 40, 125, -90, -120, 0.1, 0.1804, 0.1451);
	buatpersegipanjang(-375, -375, 40, 125, -40, -70, 0.1, 0.1804, 0.1451);
}
void dindingdepanrumah() {

	//dinding depan jendela kanan
	buatpersegipanjang(-377.5, -127.5, 20, 100, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatpersegipanjang(-377.5, -297.5, 100, 275, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatjendela(-292.5, -202.5, 105, 270, -2.5, 2.5);
	//gagang
	buatpersegipanjang(-297.5, -292.5, 100, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-292.5, -207.5, 100, 105, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-202.5, -207.5, 100, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-292.5, -207.5, 270, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-245, -255, 100, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);

	buatpersegipanjang(-207.5, -127.5, 100, 275, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatpersegipanjang(-127.5, -207.5, 275, 315, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatjendelabolongbolong(-207.5, -292.5, 275, 315, -2.5, 2.5);
	//gagang
	buatpersegipanjang(-297.5, -292.5, 275, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-292.5, -207.5, 275, 280, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-202.5, -207.5, 275, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-292.5, -207.5, 310, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);

	buatpersegipanjang(-297.5, -377.5, 275, 315, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatpersegipanjang(-127.5, -377.5, 315, 420, -2.5, 2.5, 0.40, 0.3137, 0.2627);


	//dinding depan pintu kanan kiri
	buatpersegipanjang(-127.5, -87.5, 20, 280, -2.5, 2.5, 0.40, 0.3137, 0.2627);
	buatpersegipanjang(-87.5, -77.5, 20, 280, -5, 5, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(-77.5, 77.5, 270, 280, -5, 5, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(87.5, 77.5, 20, 280, -5, 5, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(87.5, 127.5, 20, 280, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	//pintu kanan
	buatpersegipanjang(-77.5, 0, 255, 270, -5, 5, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-77.5, -62.5, 35, 270, -5, 5, 0.2275, 0.1804, 0.1451);
	buatjendela(-62.5, -15, 35, 135, -5, 5);
	buatpersegipanjang(-77.5, 0, 135, 155, -5, 5, 0.2275, 0.1804, 0.1451);
	buatjendela(-62.5, -15, 155, 255, -5, 5);
	buatpersegipanjang(-15, 0, 35, 270, -5, 5, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-77.5, 0, 20, 35, -5, 5, 0.2275, 0.1804, 0.1451);


	//pintu kiri
	buatpersegipanjang(77.5, 0, 255, 270, -5, 5, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(77.5, 62.5, 35, 270, -5, 5, 0.2275, 0.1804, 0.1451);
	buatjendela(62.5, 15, 35, 135, -5, 5);
	buatpersegipanjang(77.5, 0, 135, 155, -5, 5, 0.2275, 0.1804, 0.1451);
	buatjendela(62.5, 15, 155, 255, -5, 5);
	buatpersegipanjang(15, 0, 35, 270, -5, 5, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(77.5, 0, 20, 35, -5, 5, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-127.5, -87.5, 20, 280, -2.5, 2.5, 0.40, 0.3137, 0.2627);
	buatpersegipanjang(87.5, 127.5, 20, 280, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatpersegipanjang(-127.5, -87.5, 280, 315, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatjendelabolongbolong(-82.5, 82.5, 280, 315, -2.5, 2.5);
	//gagang
	buatpersegipanjang(-87.5, -82.5, 280, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(82.5, 87.5, 280, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(82.5, -82.5, 310, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);


	buatpersegipanjang(87.5, 127.5, 275, 315, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatpersegipanjang(-127.5, 127.5, 315, 420, -2.5, 2.5, 0.40, 0.3137, 0.2627);



	//dinding depan jendela kiri
	buatpersegipanjang(127.5, 377.5, 20, 100, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatpersegipanjang(127.5, 207.5, 100, 275, -2.5, 2.5, 0.40, 0.3137, 0.2627);
	buatjendela(207.5, 297.5, 100, 275, -2.5, 2.5);
	//gagang
	buatpersegipanjang(297.5, 292.5, 100, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(292.5, 207.5, 100, 105, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(202.5, 207.5, 100, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(292.5, 207.5, 270, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(247.5, 252.5, 100, 275, -2.6, 2.6, 0.5804, 0.4549, 0.3804);

	buatpersegipanjang(297.5, 377.5, 100, 275, -2.5, 2.5, 0.40, 0.3137, 0.2627);


	buatpersegipanjang(127.5, 207.5, 275, 315, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatjendelabolongbolong(207.5, 292.5, 275, 315, -2.5, 2.5);
	//gagang
	buatpersegipanjang(297.5, 292.5, 275, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(292.5, 207.5, 275, 280, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(202.5, 207.5, 275, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);
	buatpersegipanjang(292.5, 207.5, 310, 315, -2.6, 2.6, 0.5804, 0.4549, 0.3804);

	buatpersegipanjang(297.5, 377.5, 275, 315, -2.5, 2.5, 0.40, 0.3137, 0.2627);

	buatpersegipanjang(127.5, 377.5, 315, 420, -2.5, 2.5, 0.40, 0.3137, 0.2627);
}
void buatdinding() {
	//dinding kiri
	buatpersegipanjang(-372.5, -377.5, 20, 200, -2.5, 1002.5, 0.40, 0.3137, 0.2627);
	buatpersegipanjang(-372.5, -377.5, 320, 420, -2.5, 1002.5, 0.40, 0.3137, 0.2627);
	xawal = -2.5;
	for (int i = 0; i < 4; i++) {
		buatpersegipanjang(-372.5, -377.5, 200, 320, xawal, xawal + 65, 0.40, 0.3137, 0.2627);
		buatjendela(-372.5, -377.5, 205, 315, xawal + 70, xawal + 180);
		//gagang
		buatpersegipanjang(-372.4, -377.6, 200, 320, xawal+65, xawal + 70, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(-372.4, -377.6, 200, 320, xawal + 180, xawal + 185, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(-372.4, -377.6, 320, 315, xawal + 65, xawal + 185, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(-372.4, -377.6, 200, 205, xawal + 65, xawal + 185, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(-372.4, -377.6, 205, 315, xawal + 120, xawal + 130, 0.5804, 0.4549, 0.3804);

		buatpersegipanjang(-372.5, -377.5, 200, 320, xawal + 185, xawal + 250, 0.40, 0.3137, 0.2627);
		xawal += 250;
	}

	//dinding kanan
	buatpersegipanjang(372.5, 377.5, 20, 200, -2.5, 1002.5, 0.40, 0.3137, 0.2627);
	buatpersegipanjang(372.5, 377.5, 320, 420, -2.5, 1002.5, 0.40, 0.3137, 0.2627);
	xawal = -2.5;
	for (int i = 0; i < 4; i++) {
		buatpersegipanjang(372.5, 377.5, 200, 320, xawal, xawal + 65, 0.40, 0.3137, 0.2627);
		buatjendela(372.5, 377.5, 205, 315, xawal + 65, xawal + 185);
		//gagang
		buatpersegipanjang(372.4, 377.6, 200, 320, xawal + 65, xawal + 70, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(372.4, 377.6, 200, 320, xawal + 180, xawal + 185, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(372.4, 377.6, 320, 315, xawal + 65, xawal + 185, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(372.4, 377.6, 200, 205, xawal + 65, xawal + 185, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(372.4, 377.6, 205, 315, xawal + 120, xawal + 130, 0.5804, 0.4549, 0.3804);

		buatpersegipanjang(372.5, 377.5, 200, 320, xawal + 185, xawal + 250, 0.40, 0.3137, 0.2627);
		xawal += 250;
	}

	//dinding belakang
	buatpersegipanjang(-377.5, 377.5, 20, 200, 997.5, 1002.5, 0.40, 0.3137, 0.2627);
	buatpersegipanjang(-377.5, 377.5, 320, 420, 997.5, 1002.5, 0.40, 0.3137, 0.2627);
	xawal = -377.5;
	for (int i = 0; i < 3; i++) {
		buatpersegipanjang(xawal, xawal + 65, 200, 320, 997.5, 1002.5, 0.40, 0.3137, 0.2627);
		buatjendela(xawal + 70, xawal + 180, 205, 315, 997.5, 1002.5);
		//gagang
		buatpersegipanjang(xawal+65, xawal + 70, 200, 320, 997.5, 1002.5, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(xawal + 180, xawal + 185, 200, 320, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(xawal + 65, xawal + 185, 200, 205, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(xawal + 65, xawal + 185, 315, 320, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);
		buatpersegipanjang(xawal + 120, xawal + 130, 205, 315, 997.4, 1002.6, 0.5804, 0.4549, 0.3804);

		buatpersegipanjang(xawal + 185, xawal + 250, 200, 320, 997.5, 1002.5, 0.40, 0.3137, 0.2627);
		xawal += 250;
	}
}
void buattangga() {
	//tangga kiri
	buatpersegipanjang(-125, -110, -40, -20, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-125, -155, -40, -25, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-155, -140, -80, -40, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-155, -185, -80, -65, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-185, -170, -120, -80, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-185, -215, -120, -115, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-215, -200, -160, -120, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-215, -245, -160, -145, -250, -450, 0.2275, 0.1804, 0.1451);

	//tengah tangga kiri
	buatpersegipanjang(-245, -230, -200, -160, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-245, -355, -200, -185, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-230, -355, -240, -200, -435, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-230, -355, -240, -225, -480, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-230, -355, -280, -240, -465, -480, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-230, -355, -280, -265, -510, -480, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-230, -355, -320, -280, -495, -510, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-230, -355, -320, -305, -540, -510, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-230, -355, -360, -320, -525, -540, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-230, -355, -360, -345, -570, -540, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(-230, -355, -400, -360, -555, -570, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(-230, -355, -400, -385, -600, -570, 0.2275, 0.1804, 0.1451);

	//tangga kanan
	buatpersegipanjang(125, 110, -40, -20, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(125, 155, -40, -25, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(155, 140, -80, -40, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(155, 185, -80, -65, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(185, 170, -120, -80, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(185, 215, -120, -115, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(215, 200, -160, -120, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(215, 245, -160, -145, -250, -450, 0.2275, 0.1804, 0.1451);

	//tengah tangga kanan
	buatpersegipanjang(245, 230, -200, -160, -250, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(245, 355, -200, -185, -250, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(230, 355, -240, -200, -435, -450, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(230, 355, -240, -225, -480, -450, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(230, 355, -280, -240, -465, -480, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(230, 355, -280, -265, -510, -480, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(230, 355, -320, -280, -495, -510, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(230, 355, -320, -305, -540, -510, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(230, 355, -360, -320, -525, -540, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(230, 355, -360, -345, -570, -540, 0.2275, 0.1804, 0.1451);

	buatpersegipanjang(230, 355, -400, -360, -555, -570, 0.2275, 0.1804, 0.1451);
	buatpersegipanjang(230, 355, -400, -385, -600, -570, 0.2275, 0.1804, 0.1451);
}
void summontiang() {
	//tiang
	//tiang 1
	buatsilinder(-375, -650, -400, 15, 400);
	buatsilinder(375, -650, -400, 15, 400);
	buatsilinder(-375, -650, -7.5, 15.5, 7.5);
	buatsilinder(375, -650, -7.5, 15.5, 7.5);
	buatsilinder(-375, -650, 0, 16.5, 7.5);
	buatsilinder(375, -650, 0, 16.5, 7.5);
	buatkotak(-375, -650, 0, 22.5, 420.1);
	buatkotak(375, -650, 0, 22.5, 420.1);

	//tiang tengah 1
	buatsilinder(-125, -650, -400, 15, 400);
	buatsilinder(125, -650, -400, 15, 400);
	buatsilinder(-125, -650, -7.5, 15.5, 7.5);
	buatsilinder(125, -650, -7.5, 15.5, 7.5);
	buatsilinder(-125, -650, 0, 16.5, 7.5);
	buatsilinder(125, -650, 0, 16.5, 7.5);
	buatkotak(-125, -650, 0, 22.5, 420.1);
	buatkotak(125, -650, 0, 22.5, 420.1);

	//tiang 2
	buatkotak(-375, -250, -400, 22.5, 400);
	buatkotak(375, -250, -400, 22.5, 400);
	buatkotak(-375, -250, 0, 22.5, 420.1);
	buatkotak(375, -250, 0, 22.5, 420.1);

	//tiang tengah 2
	buatkotak(-125, -250, -400, 22.5, 400);
	buatkotak(125, -250, -400, 22.5, 400);
	buatkotak(-125, -250, 0, 22.5, 420.1);
	buatkotak(125, -250, 0, 22.5, 420.1);

	for (int i = 0; i < 5; i++) {
		//tiang 4 - 6
		buatkotak(-375, i * 250, -400, 22.5, 400);
		buatkotak(375, i * 250, -400, 22.5, 400);
		buatkotak(-375, i * 250, 0, 22.5, 420.1);
		buatkotak(375, i * 250, 0, 22.5, 420.1);

		//tiang tengah 4 - 6
		buatkotak(-125, i * 250, -400, 22.5, 400);
		buatkotak(125, i * 250, -400, 22.5, 400);
	}
}

void display() {
	glPushMatrix();
	glScalef(0.2, 0.2, 0.2);
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	gluLookAt(0, 1000, -1000, 0, 0, 0, 0, 1, 0);
	glRotatef(180, 0, 1, 0);
	glRotatef(-90, 1, 0, 0);
	glRotatef(xrot, 1, 0, 0);
	glRotatef(yrot, 0, 1, 0);



	bawahan();
	pagar();
	summontiang();
	buatdinding();
	dindingdepanrumah();
	buattangga();
	atep();
	atasnyaatep();
	glPopMatrix();
	glFlush();
}
void idle()
{
	if (!mouseDown)
	{
		xrot += 0.3f;
		yrot += 0.4f;
	}
	glutPostRedisplay();
}
bool mousedown = false;
void mouse(int button, int state, int x, int y) {
	if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN) {
		mousedown = true;
		xdiff = x - yrot;
		ydiff = -y + xrot;
	}
	else {
		mousedown = false;
	}

}
void mousemotion(int x, int y) {
	if (mousedown) {
		yrot = x - xdiff;
		xrot = y + ydiff;
		glutPostRedisplay();
	}
}
void keyboard(unsigned char key, int x, int y) {
	switch (key) {
	case 'a':
	case 'A':
		glTranslatef(-1.0, 0.0, 0.0);
		break;
	case 'd':
	case 'D':
		glTranslatef(1.0, 0.0, 0.0);
		break;
		//gerak ke depan
	case 'w':
	case 'W':
		glTranslatef(0.0, 1.0, 0);
		break;
		//gerak ke belakang
	case 's':
	case 'S':
		glTranslatef(0.0, -1.0, 0);
		break;
		//gerak ke atas
	case 'q':
	case 'Q':
		glTranslatef(0.0, 0, 1.0);
		break;
		//gerak ke bawah
	case 'e':
	case 'E':
		glTranslatef(0.0, .0, -1.0);
		break;
		//rotate ke kiri
	case 'j':
	case 'J':
		glRotatef(1.0, 0.0, -5.0, 0.0);
		break;
		//rotate ke kanan
	case 'l':
	case 'L':
		glRotatef(1.0, 0.0, 5.0, 0.0);
		break;
		//rotate ke samping kanan
	case 'o':
	case 'O':
		glRotatef(1.0, 0.0, 0.0, -5.0);
		break;
		//rotate ke samping kiri
	case 'u':
	case 'U':
		glRotatef(1.0, 0.0, 0.0, 5.0);
		break;
		//rotate ke atas
	case 'i':
	case 'I':
		glRotatef(1.0, -5.0, 0.0, 0.0);
		break;
		//rotate ke bawah
	case 'k':
	case 'K':
		glRotatef(1.0, 5.0, 0.0, 0.0);
		break;
	}
	display();
	//glutPostRedisplay(); 
}
void ukuran(int lebar, int tinggi)
{
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	glViewport(0, 0, lebar, tinggi);
	gluPerspective(50.0, lebar / tinggi, 5.0, 1500);
	glTranslatef(0.0, -5.0, -150.0);
	glMatrixMode(GL_MODELVIEW);
}
int main(int argc, char** argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGBA | GLUT_DEPTH | GLUT_SINGLE);
	glutInitWindowSize(1050, 700);
	glutInitWindowPosition(210, 30);
	glutCreateWindow("672016157");
	initRendering();
	glutDisplayFunc(display);
	glutReshapeFunc(ukuran);

	glutMouseFunc(mouse);
	glutMotionFunc(mousemotion);
	glutKeyboardFunc(keyboard);


	//glutIdleFunc(display2);
	Inisialisasi();
	glutMainLoop();
	return 0;
}

