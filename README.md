# SnakeGame
Kodladığım ilk projem.

AnaPencere Sınıfı

import java.awt.Dimension;
import java.awt.Toolkit;
import javax.swing.JFrame;

public final class AnaPencere extends JFrame {
    
    
    private final int mGenislik;
    private int mYukseklik = 600;
   
    private static AnaPencere mPencere =null;
    
    private AnaPencere()
    { 
        this.mGenislik = 600;
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setDimension(mGenislik, mYukseklik );
        setResizable(false);
        
      yılan Y = new yılan();
      
      add(Y);
      
    }

    public static AnaPencere PencereGetir()
     {       
     
         if (mPencere==null)
            mPencere = new AnaPencere();
        
         return mPencere;
         
     }
       public void setDimension(int Genislik,int Yukseklik)
      {
         Dimension Dim = Toolkit.getDefaultToolkit().getScreenSize();
         
         int PosX = 0;
         int PosY = 0;
         
         if(mGenislik+100> Dim.width)
             Genislik = Dim.width-100;
         if(mYukseklik+100> Dim.height)
             mYukseklik = Dim.height-100;
         
         PosX = (Dim.width-mGenislik)/2;
         PosY = (Dim.height-mYukseklik)/2;
         
         setBounds(PosX, PosY, mGenislik, mYukseklik);
     }
         
         
                
    }

    
    Yılanı Oluşturan Kutuların Sınıfı
    /*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package snakegame;

import java.awt.BasicStroke;
import java.awt.Color;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.geom.Rectangle2D;
import javax.swing.JLabel;

/**
 *
 * @author ORDULUPIATTIEASTWOOD
 */
public class kutu extends JLabel{
    public int mGenislik = 20;   
    public int mYon      = YON.SAG;
   kutu()
   {        
       setBounds(100, 100, mGenislik, mGenislik);
   }
    @Override
   public void paint(Graphics g)
   {
        
        Graphics2D g2 = (Graphics2D)g;
     
        Rectangle2D rect = new Rectangle2D.Double(1, 1, getWidth()-2, getHeight()-2);
        
        g2.setColor(Color.black);
        
        g2.setStroke(new BasicStroke(2));
        
        g2.draw(rect); 
        
        g2.setColor(Color.red);
        
        g2.fill(rect);  
   }
   public void SolaGit()
   {
       int PosX = getX();
       int PosY = getY();
             
       PosX-=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   }
   public void SagaGit()
   {
        int PosX = getX();
       int PosY = getY();
             
       PosX+=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   } 
      public void YukariGit()
   {
        int PosX = getX();
       int PosY = getY();
             
       PosY-=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   } 
      public void AsagiGit()
   {
        int PosX = getX();
       int PosY = getY();
             
       PosY+=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   } 
      public kutu KutuOlustur()
      {
          kutu K = new kutu();
          
          int X = getX();
          int Y = getY();
          
          K.setBounds(X,Y,mGenislik,mGenislik);
          
          K.mYon = -mYon;
          
          K.Hareket();
          
          K.mYon = mYon;
          
          
          return K;
      }
      public void Hareket()
      {
          if (mYon == YON.SOL)
              SolaGit();
          else if(mYon == YON.SAG)
              SagaGit();
          else if(mYon == YON.YUKARI)
              YukariGit();
          else if(mYon == YON.ASAGI) 
              AsagiGit();
      }
}

Yılana Yön Veren Interface

package snakegame;

/**
 *
 * @author ORDULUPIATTIEASTWOOD
 */
public interface YON 
{
    public static final int SOL = 1; 
    public static final int ASAGI = -2;
    public static final int YUKARI = 2;
    public static final int SAG = -1;
    
}

Yılan'ın Kodlanması

package snakegame;

/**
 *
 * @author ORDULUPIATTIEASTWOOD
 */
public interface YON 
{
    public static final int SOL = 1; 
    public static final int ASAGI = -2;
    public static final int YUKARI = 2;
    public static final int SAG = -1;
    
}
