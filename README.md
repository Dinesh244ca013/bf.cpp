#include <GL/glut.h>

float boundary[] = {1,0,0}; // Red
float fill[] = {0,1,0};     // Green

void getPixel(int x,int y,float c[3]){
    glReadPixels(x,y,1,1,GL_RGB,GL_FLOAT,c);
}

void setPixel(int x,int y){
    glColor3fv(fill);
    glBegin(GL_POINTS);
    glVertex2i(x,y);
    glEnd();
    glFlush();
}

void boundaryFill(int x,int y){
    float c[3];
    getPixel(x,y,c);

    if((c[0]!=boundary[0]||c[1]!=boundary[1]||c[2]!=boundary[2]) &&
       (c[0]!=fill[0]||c[1]!=fill[1]||c[2]!=fill[2])){

        setPixel(x,y);

        // 8-connected
        boundaryFill(x+1,y); boundaryFill(x-1,y);
        boundaryFill(x,y+1); boundaryFill(x,y-1);
        boundaryFill(x+1,y+1); boundaryFill(x-1,y+1);
        boundaryFill(x+1,y-1); boundaryFill(x-1,y-1);
    }
}

void display(){
    glClear(GL_COLOR_BUFFER_BIT);

    glColor3f(1,0,0);
    glBegin(GL_LINE_LOOP);
    glVertex2i(100,100);
    glVertex2i(300,100);
    glVertex2i(300,300);
    glVertex2i(100,300);
    glEnd();

    glFlush();
}

void mouse(int btn,int state,int x,int y){
    if(btn==GLUT_LEFT_BUTTON && state==GLUT_DOWN)
        boundaryFill(x,500-y);
}

int main(int argc,char** argv){
    glutInit(&argc,argv);
    glutInitWindowSize(500,500);
    glutCreateWindow("Boundary Fill");
    gluOrtho2D(0,500,0,500);

    glutDisplayFunc(display);
    glutMouseFunc(mouse);

    glutMainLoop();
}