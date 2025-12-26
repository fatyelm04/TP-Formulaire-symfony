# TP-Formulaire-symfony
construction du formulaire en Symfony avec FormType
# L'objectif du TP
L’objectif de ce TP est de transformer un formulaire HTML statique en un formulaire Symfony
en utilisant les bonnes pratiques du framework, notamment :
- Twig pour l’affichage des vues,
- le FormBuilder et les FormTypes pour la construction du formulaire,
    $productForm = $this->createForm(ProductType::class);
- le Controller pour la logique de traitement et la validation du formulaire
 
#  Travail réalisé
Dans le cadre de ce TP, les étapes suivantes ont été réalisées :

Création d’un FormType (ProductType)  permettant la gestion et la validation du formulaire associé au produit.   Form/Type/ProductType.php 


                          `<?php
                  declare (strict_types=1);
                  namespace App\Form\Type;
                  
                  use BcMath\Number;
                  use Symfony\Component\Form\AbstractType;
                  use Symfony\Component\Form\Extension\Core\Type\ChoiceType;
                  use Symfony\Component\Form\Extension\Core\Type\IntegerType;
                  use Symfony\Component\Form\Extension\Core\Type\NumberType;
                  use Symfony\Component\Form\Extension\Core\Type\SubmitType;
                  use Symfony\Component\Form\FormBuilderInterface;
                  use Symfony\Component\Validator\Constraints\Choice;
                  
                  class ProductType extends AbstractType
                  {
                      public function buildForm(FormBuilderInterface $builder, array $options):void
                      {
                         $builder->add('quantite', IntegerType::class, [
                                              'label' => 'Quantité',
                                              'required' => true,
                                              'data' => 1,
                                              'attr'=>[
                                                  'class'=>'form-control',
                                                  'min'=>1,
                                                  'class' => 'mb-3',
                                                  'style' => 'max-width: 70px;
                                                  display:flex;
                                              flex-direction:column;',
                                              ]])
                  
                  
                                  ->add('color',type:ChoiceType::class, options:[
                                          'choices' =>[
                                          'Matte Black' => 'matte_black',
                                          'Pearl White' => 'pearl_white',
                                          'Silver' => 'silver'],
                                          'attr'=>[
                                              'class' => 'mb-3',
                                              'style' => 'max-width: 150px;
                                              display:flex;
                                              flex-direction:column',
                  
                                          ]
                  
                                  ])
                                 ->add('submit', SubmitType::class,[
                                          'label' => 'Add to card',
                                         'attr' => [
                                          'class' => 'btn btn-primary',]
                                         ]);
                      }
                  
                  }
                  
                  ?>
`
        
                         


Mise en place d’un contrôleur (ProductController) chargé de l’affichage du formulaire et du traitement des données soumises.
              `` <?php
              
              namespace App\Controller;
              
              use App\Form\Type\ProductType;
              use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
              use Symfony\Component\HttpFoundation\Response;
              use Symfony\Component\Routing\Attribute\Route;
              use Symfony\Component\OptionsResolver\OptionsResolver;
              
              final class ProductController extends AbstractController
              {
                  #[Route('/product', name: 'app_product')]
                  public function index(): Response
                  {
              
                      $productForm = $this->createForm(ProductType::class);
                      return $this->render('product/index.html.twig',
                      ['product' => $productForm,]);
                  }
                  public function configureOptions(OptionsResolver $resolver): void
                  {
                  $resolver->setDefaults([
                      'csrf_protection' => true,
                      'csrf_field_name' => '_product_token',
                      'csrf_token_id' => 'product',
                  ]);
                  }
              }
            ``




Utilisation du moteur de templates Twig pour intégrer et afficher le formulaire au sein d’une page HTML.
                  
                  ``{% extends 'base.html.twig'%}
                  {% block body %}
                  <div class="container my-5">
                  <div class="row">
                      <div class="col-md-6">
                      <img src="https://images.pexels.com/photos/90946/pexels-photo-90946.jpeg?auto=compress&cs=tinysrgb&w=800" class="img-fluid rounded" alt="Product">
                      </div>
                  
                      <div class="col-md-6">
                      <h1 class="mb-3">Premium Wireless Headphones</h1>
                      <p class="text-muted fs-4 mb-3">$129.99</p>
                  
                      <p class="mb-4">
                          Experience superior sound quality with our premium wireless headphones.
                          Features active noise cancellation, 30-hour battery life, and premium comfort padding.
                      </p>
                  
                      <ul class="list-unstyled mb-4">
                          <li><strong>Brand:</strong> AudioTech</li>
                          <li><strong>Color:</strong> Matte Black</li>
                          <li><strong>Connectivity:</strong> Bluetooth 5.0</li>
                          <li><strong>Battery Life:</strong> 30 hours</li>
                      </ul>
                  
                      {{form_start(product)}}
                      <div>
                           {{form_row(product.quantite)}}
                      </div>
                      <div>
                           {{form_row(product.color)}}
                      </div>
                      <div>
                           {{form_row(product.submit)}}
                      </div>
                  
                  {{form_end(product)}}
                  
                  
                      </div>
                  </div>
                  </div>
                  
                  
                  {% endblock %}``


Intégration du framework Bootstrap 5.3 afin d’assurer un rendu visuel conforme aux exigences de style demandées.<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">

  





